---

# Java8 이전

--

## Guest 예제

    public class Guest {
        private final int grade;
        private final String name;
        private final String company;
    ...
	}

( [Guest.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/domain/Guest.java) )

    public interface GuestRepository {
        public List<Guest> findAllGuest ();
    }

( [GuestRepository.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/repository/GuestRepository.java) )

--

ReservationRepository.findAllGuest 호출 결과를

- (A) 특정 company인 guest를 필터링해서
- (B) grade값의 오름차순으로 정렬하여
- (C) name만 추출하여 List<String>으로 변환

--

### Classic Java
한번에 짜면 너무 호흡이 길어지니 메서드를 나눠서

	public List<String> findGuestNamesOfCompany(String company) {
		List<Guest> all = repository.findAllGuest();

		List<Guest> filtered = filter(guests, company); // (A)
		sort(filtered); // (B)
		return mapNames(filtered); // (C)
	}

([ClassicJavaService.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/ClassicJavaService.java))

--

(A) 특정 company인 Guest만 Filterting

	private List<Guest> filter(List<Guest> guests, String company) {
		List<Guest> filtered = new  ArrayList<>();
		for(Guest guest : guests ) {
			if (company.equals(guest.getCompany())) {
				filtered.add(guest);
			}
		}
		return filtered;
	}

--

(B) grade값의 오름차순으로 Sorting

	private void sort(List<Guest> guests) {
		Collections.sort(guests, new Comparator<Guest>() {
			@Override
			public int compare(Guest o1, Guest o2) {
				return Integer.compare(o1.getGrade(), o2.getGrade());
			}
		});
	}


--

(C) name만 추출하여 List<String>으로 Mapping

	private List<String> mapNames(List<Guest> guests) {
		List<String> names = new ArrayList<>();
		for(Guest guest : guests ) {
			names.add(guest.getName());
		}
		return names;
	}

--

### 핵심행위
- (A) Filtering : company.equals(guest.getCompany()
- (B) Sorting :  Integer.compare(o1.getGrade(), o2.getGrade())
- (C) Mapping : guest.getName()


--

### 람다가 있었다면?
- 핵심 행위만 기술하기가 더 쉽다.
- Lambda : 익명함수
	- Closure는 자유변수를 접근할 수 있는 코드
- 하지만 Java는 람다 도입에 보수적이였다.
    - 익명 클래스로 만족하고 살 수 있을까?

--

기존 Java문법으로 람다와 같은 일을 하는 라이브러리를 살펴보자

--

## 람다를 동경한 라이브러리

### [Bolt](https://bitbucket.org/stepancheg/bolts/wiki/Home)

	public List<String> findGuestNamesOfCompany(final String company) {
		List<Guest> all = repository.findAllGuest();
		// filter
		ListF<Guest> filtered = Cf.list(all)
			.filter(new Function1B<Guest>() {
				public boolean apply(Guest g) {
					return company.equals(g.getCompany());
				}
			});

		// sort
		filtered.sort(new Comparator<Guest>() {
			public int compare(Guest o1, Guest o2) {
				return Integer.compare(o1.getGrade(), o2.getGrade());
			}
		});

		// map
		ListF<String> names = filtered.map(new Function<Guest, String>() {
			public String apply(Guest g) {
				return g.getName();
			}
		});
		return names.toList();
	}

( [BoltService.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/BoltService.java) )


--

- 함수 인터페이스 : Function 인터페이스 + 익명 클래스로 함수 표현
- 함수친화적 자료구조 :  ListF
- Fluent API
	- void형의 sort가 중간에 들어가서 위에서는 표현 못했음

--

### [Op4j](www.op4j.org/)

	public List<String> findGuestNamesOfCompany(final String company) {
		List<Guest> all = repository.findAllGuest();
		return Op.on(all)
			.removeAllFalse(new IFunction<Guest, Boolean>() {
				public Boolean execute(Guest g, ExecCtx ctx) throws Exception {
					return company.equals(g.getCompany());
				}
			})
			.sort(new Comparator<Guest>() {
				public int compare(Guest o1, Guest o2) {
					return Integer.compare(o1.getGrade(), o2.getGrade());
				}
			})
			.map(new IFunction<Guest, String>() {
				public String execute(Guest g, ExecCtx ctx) throws Exception {
					return g.getName();
				}
			}).get();
	}

( [Op4JService.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/Op4JService.java) )

--

- 함수 인터페이스 :IFunction
- 함수친화적 자료 구조 : Level0ListOperator
- Fluent API

--

### [Guava](https://code.google.com/p/guava-libraries/)

	public List<String> findGuestNamesOfCompany(final String company) {
		List<Guest> all = repository.findAll();

		// filter and sort
		ImmutableList<Guest> sorted = FluentIterable.from(all)
			.filter(new Predicate<Guest>() {
				public boolean apply(Guest g) {
					return company.equals(g.getCompany());
				}
			})
			.toSortedList(new Comparator<Guest>() {
					public int compare(Guest o1, Guest o2) {
						return Integer.compare(o1.getGrade(), o2.getGrade());
					}
			});
		 return FluentIterable.from(sorted).transform(new Function<Guest, String>() {
			 public String apply(Guest g) {
				return g.getName();
			}
		}).toList();
	}

( [GuavaService.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/GuavaService.java) )

--

- 함수 인터페이스:  Function, Predicate
- Fluent API

--

### [Lambda J](https://code.google.com/p/lambdaj/)

    import static ch.lambdaj.Lambda.*;
    import static org.hamcrest.Matchers.*;
    ...

	public List<String> findGuestNamesOfCompany(String company) {
		List<Guest> all = repository.findAllGuest();
		List<Guest> filtered = filter(having(on(Guest.class).getCompany(), equalTo(company)), all);
		List<Guest> sorted = sort(filtered, on(Guest.class).getGrade());
		return convert(sorted, new PropertyExtractor<Guest, String>("name"));
	}

( [LambdaJService.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/LambdaJService.java) )

--

- 함수 인터페이스 : Converter<F, T>
	- static import와 factory 메서드로 효율화 (Hamcrest matcher)
	- 결과코드는 이쁘지만 적절한 메서드를 찾는게 쉽지는 않았음

--

### [FunctionalJava](http://functionaljava.org/)

	public List<String> findGuestNamesOfCompany(String company) {
		List<Guest> all = repository.findAll();

		Collection<String> mapped = Stream.iterableStream(all)
			.filter(new F<Guest, Boolean>() {
				public Boolean f(Guest g){
					return company.equals(g.getCompany());
				}
			})
			.sort(Ord.ord(
				new F<Guest, F<Guest, Ordering>>() {
					public F<Guest, Ordering> f(final Guest a1) {
						return new F<Guest, Ordering>() {
							public Ordering f(final Guest a2) {
								int x =  Integer.compare(a1.getGrade(), a2.getGrade());
								return x < 0 ? Ordering.LT : x == 0 ? Ordering.EQ : Ordering.GT;
							}
						};
					}
			}))
			.map(new F<Guest, String>() {
				@Override
				public String f(Guest g) {
					return g.getName();
				}
			})
			.toCollection();
		return new ArrayList<String>(mapped);
	}

( [FunctionalJavaService.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/FunctionalJavaService.java) )

--

- 함수 인터페이스 : F
- 함수친화적 자료 구조 : Stream
- Fluent API
- 특이하게 여러개의 파라미터를 가지는 함수는 F를 여러번 합성해서
	- 더 복잡해 보인다

--

### 반복적으로 보이는 요소
- 함수를 표현하는 인터페이스
	- 주로 익명클래스로 사용
    - 편의를 제공하는 Factory method + static import가  쓰이기도
- Fluent API
- 함수를 적용하기 쉬운 새로운 List와 유사한 자료 구조

--

기존 문법에서 최선을 다하고 있지만 아무래도 장황하다.

문법 제약에서 자유로운 JVM의 다른 언어는 어떻게 표현할까?

--

## 다른 JVM 언어의 람다

### [Groovy](http://groovy.codehaus.org/)

	allGuests
    	.findAll { g.company == company }
    	.sort { g.grade }
        .collect { g.name }

--

### [Scala](http://www.scala-lang.org/)

	allGuest filter {_.company == company} sortBy {_.grade} map {_.name}

--

### [Kotlin](http://kotlinlang.org/)

	allGuests
    	.filter { g.company == company }
    	.sortBy { g.grade}
	    .map { g.name }

--

### [Xtent](http://www.eclipse.org/xtend/)

    allGuests.stream
          .filter[company == company]
          .sorted[comparing[name]]
          .map[name]

--

### [Ceylon](http://ceylon-lang.org/)

	allGuests
    	.filter((Guest g) g.company == company)
    	.sort(byIncreasing((Guest g) => g.grade))
    	.map((Guest c) g.name);

--


## 문법 차원의 지원이 절실하다

Mark Jason Dominus

> 지금 '회귀호출'이 없는 언어를 발명하려는 사람을 비웃듯이, 앞으로 30년 안에는 'closure'가 없는 언어를 발명하려는 사람은 비웃음을 사게 될 것이다.

> (In another thirty years people will laugh at anyone who tries to invent a language without closures, just as they'll laugh now at anyone who tries to invent a language without recursion. )

( Interview with Mark Jason Dominus. The Perl Review (April 7, 2005). )


--

- 익명클래스도 제한된 Closure역할을 할 수는 있지만, 표현식이 장황하다
- 다른 주요 언어는 람다표현식을 다 도입했다.
	- C# : 2007
    - Objective C : 2010
    - C++ : 2011

---

# Java8의 선택

--

## Java8의 함수 인터페이스

### Guest 예제

	public List<String> findGuestNamesOfCompany(String company) {
		List<Guest> guests = repository.findAll();
		return guests.stream()
			.filter(g -> company.equals(g.getCompany()))
			.sorted(Comparator.comparing(Guest::getGrade))
			.map(g -> g.getName())
			.collect(Collectors.toList());
	}

( [ModernJavaService.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/ModernJavaService.java) )

--

#### 핵심행위의 표현
- (A) Filtering : g -> company.equals(g.getCompany())
- (B) Sorting :  Comparator.comparing(Guest::getGrade)
- (C) Mapping : g -> g.getName()
- 핵심 행위를 앞뒤 다른 선언없이 바로 매개변수로 넘길 수 있다

--

#### 특징
- 함수 인터페이스 : 잘라서 타입을 보면 알수 있음
- 함수 인터페이스를 받아주는 자료구조
	- Stream
    - Collection.forEach 같은 디폴트 메서드 추가
- Fluent API
- 문법 지원 : "->"
	- C#과 Scala에서는 "=>"

--

#### 잘라서 타입을 보면

	public List<String> findGuestNamesOfCompany(String company) {
		List<Guest> all = repository.findAll();

		Stream<Guest> stream = all.stream();

		// filtering
		Predicate<Guest> filterFunc = g -> company.equals(g.getCompany());
		Stream<Guest> filtered = stream.filter(filterFunc);

		// sorting
		Comparator<Guest> sortFunc = Comparator.comparing(Guest::getGrade);
		Stream<Guest> sorted = filtered.sorted(sortFunc);

		// mapping
		Function<Guest, String> mapFunc = g -> g.getName();
		Stream<String> mapped = sorted.map(mapFunc);
		return mapped.collect(Collectors.toList());
	}

( [ModernJavaBreak2Service.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/ModernJavaBreak2Service.java) )

--

### 함수 인터페이스의 도입 (Function Interface)

    public interface Supplier<T> {
        T get();
    }
    public interface Predicate<T> {
        boolean test(T t); ...
    }
    public interface Function<T, R> {
        R apply(T t); ...
    }
	public interface BiFunction<T, U, R> {
    	R apply(T t, U u); ...
    }
	public interface Consumer<T> {
    	void accept(T t) ...
	}
	public interface BiConsumer<T, U> {
		void accept(T t, U u); ..
	}

--


- java.util.function 패키지
    - 아주 많다
    	- <http://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html>
- Bolt, Guava같은 라이브러리의 접근 방식과 유사
	- 함수는 여전히 1급시민이 아니다
- 메서드의 파라미터만 맞다면 람다식으로 바로 대입가능


	@FunctionalInterface
	public static interface StringAction {
	    void execute(String str);
	}
	...
	Consumer<String> f1 = System.out::println;
	StringAction f2 = System.out::println;
	StringAction f3 =  s ->  System.out.println ("!!" + s);

--

## 내부 구현

### 왜 새로운 함수형을 도입하지 않았는가?
- JVM has no native representation of function type in VM type signatures
- Forward compatible
	- 이전 라이브러리로도 람다식의 장점을 누릴수 있도록


--

> 우리가 자바에 클로저를 더 한다면 그것은 이미 지원되고 있는 문법의 테두리 안에서 조심스럽게 이루어져야할 것입니다.

> 즉, 클로저가 내부에 하나의 메서드만 가지고 있는 인터페이스를 구현하는 형태를 가져야한다는 뜻입니다.  Runnable 같은 인터페이스나 TimerTask같은 클래스처럼 말입니다. 이미 존재하는 익명 클래스 문법에 약간의 수정을 가할 필요도 있습니다.

[Joshua Bloch과의 2006년 인터뷰](http://www.infoq.com/interviews/joshua-bloch) 중.( 번역은 폴리갓프로그래밍(임백준 저) 64쪽.)

--

> I think the most important thing is to make it easier to do what you can already do with anonymous inner classes, just to remove all of that needless verbosity.

[Joshua Bloch과의 2010년 인터뷰](http://www.infoq.com/interviews/josh-bloch-java-prog) 중.

--

### InvokeDynamic을 활용

#### InvokeDynamic이란?
- 원래 JDK7에서 JVM위의 다른 언어를 위해 들어간 바이트코드 명세
- 메서드 실행을 위한 다른 바이트 코드
	- invokestatic:  static methods
	- invokevirtual:  class methods
	- invokeinterface: interface methods
	- invokespecial:  constructors, private methods, and super-calls

--

#### 람다식의 선택
- 람다식의 실행은 InvokeDynamic 활용
	- javap로 바이트코드 확인가능
- 성능 이점
	-  가장 큰 차이가 나는 실험케이스에서는 익명클래스를 쓸 때에 비해서 1/67의 인터턴스 생성 비용 (capturing cost)
		- [Lambda peek under the hood](http://www.infoq.com/presentations/lambda-invokedynamic)의 37쪽
- 검토되던 다른 방식은 깔끔하지 않았다
	- 익명클래스
	- MethodHandle

---

# 활용 예시

--

## 어플리케이션 코드

### Async Servet
#### Classic java

	public void doGet(final HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		final AsyncContext asyncContext = request.startAsync();
		asyncContext.start(new Runnable() {
			@Override
			public void run() {
				// 오래 걸리는 작업
				asyncContext.dispatch("/threadNames.jsp");
			}
		});
	}

( [ClassicAsyncServlet.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/servlet/ClassicAsyncServlet.java) )

--

#### Modern Java

	@Override
	public void doGet(final HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		final AsyncContext asyncContext = request.startAsync();
		asyncContext.start(() -> {
			// 오래 걸리는 작업
			asyncContext.dispatch("/threadNames.jsp");
		});
	}

( [ModernAsyncServlet.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/servlet/ModernAsyncServlet.java) )

--

### Spring JdbcTemplate

#### Classic Java

	public List<Guest> findAll() {
		return jdbc.query(SELECT_ALL, new RowMapper<Guest>(){
			@Override
			public Guest mapRow(ResultSet rs, int rowNum) throws SQLException {
				return  new Guest (
						rs.getInt("id"), 
						rs.getString("name"), 
						rs.getString("company"),
						rs.getInt("grade")
						);
			}
		});
	}

( [ClassicJdbcRepository.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/repository/ClassicJdbcRepository.java) )

--

#### Modern Java

	public List<Guest> findAll() {
		return jdbc.query(SELECT_ALL, 
			(rs, rowNum) ->new Guest (
					rs.getInt("id"), 
					rs.getString("name"), 
					rs.getString("company"),
					rs.getInt("grade")
			)
		);
	}

( [ModernJdbcRepository.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/repository/ModernJdbcRepository.java) )

--

### Android Event처리

#### jquery 였다면

    function calculate() { ... }
    function send() { ... }
    ...
    $("#calcButton").bind("click", calculate);
    $("#sendButton").bind("click", send);


--

#### Classic java Android

    Button calcButton = (Button) view.findViewById(R.id.calcBtn);
    Button sendButton = (Button) view.findViewById(R.id.sendBtn);

    calcButton.setOnClickListener(new OnClickListener() {
        @Override
        public void onClick(View view) {
            calculate();
        }
    });
    sendButton.setOnClickListener(new OnClickListener() {
        @Override
        public void onClick(View view) {
            send();
        }
    });

( [ClassicFragment.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/android/ClassicFragment.java) )

--

#### Modern java Android

    Button calcButton = (Button) view.findViewById(R.id.calcBtn);
    Button sendButton = (Button) view.findViewById(R.id.sendBtn);

	calcButton.setOnClickListener(v -> calculate());
	sendButton.setOnClickListener(v -> send());


( [ModernFragment.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/android/ModernFragment.java) )

--

## 프레임워크

### [Jinq](http://www.jinq.org/)

	public List<String> findGuestNamesOfCompany(String company) {
		return guests()
			.where(g -> g.getCompany().equals(company))
			.sortedByIntAscending(g -> g.getGrade())
			.select(g -> g.getName())
			.toList();
	}

	private JinqStream<Guest> guests() {
		return new JinqJPAStreamProvider(em.getEntityManagerFactory()).streamAll(em, Guest.class);
	}

( [JinqService.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/service/JinqService.java) )

--

람다와 스트림을 이용한 SQL 생성

	select ....
    from guest guest0_
    where guest0_.company =? limit ?


'where' 절이 들어간 SQL이 생성된다. ( [JinqServiceRun.java](http://yobi.navercorp.com/sanghyuk.jung/lambda-resort/code/master/src/main/java/com/naver/helloworld/resort/JinqServiceRun.java)를 실행하고 Console 확인 )


--

### [Spark](http://www.sparkjava.com/)
Ruby의 Sinatra와 유사한 Micro webframework

    public class FilterExample {

       public static void main(String[] args) {
         get("/hello/:name", (request, response) -> {
            return "Hello: " + request.params(":name");
         });
       }
    }

---

# 시사점

--

## Java의 2차 언어 혁명
- 1차 (JDK 5): Generics, Annotation, Enum
- 2차 (JDK 8): Lambda, Default Method ...

1차 혁명의 산물

	@ResponseBody
    @RequestMapping(value="/issue/{id}", method=RequestMethod.GET)
    public ResponseEntity<Issue> issue(@PathVaraible Integer id) {
		...
    }

--

## 시대의 요구
- 이벤트 처리, 비동기/병렬 처리 처리
	- 다양한 Client 형태로 비동기 처리가 늘어남
    - Internal Iteration이 병렬처리에 더 유리
    	- Collection.forEach
- 기존 표현식의 번거로움이 더욱 더 드러난다

--

## 기회이자 숙제
- 축약된 사고를 도와준다
- 더 나은 API들이 쏟아져 나올 것이다
- 라이브러리 설계자는 더 많은 고민을 해야할 것이다.

--

# 참고자료
- [JSR 335]() JSR의 람다 관련 명세 : <https://www.jcp.org/en/jsr/detail?id=335>
- Java8 람다의 어려운 점 :  <http://blog.jooq.org/2014/04/04/java-8-friday-the-dark-side-of-java-8/>
- Effective Java의 저자 Joshua Bloch의 의견
	- 2006년 인터뷰 : <http://www.infoq.com/interviews/joshua-bloch>
    - 2010년 인터뷰 : <http://www.infoq.com/interviews/josh-bloch-java-prog>
- Lambda peek under the hood  (Java언어 아키텍트인 Brian Goetz의 발표) : <http://www.infoq.com/presentations/lambda-invokedynamic>
- Eclipsecon 2004의 'The Road To Lambda' : <https://www.eclipsecon.org/na2014/sites/default/files/slides/2014-03-18%20The%20Road%20To%20Lambda.pdf>
- Devox 2013의 Invoke Dyamic 관련 발표 : <http://www.parleys.com/play/517af3a5e4b0736a5fa66a20/chapter0/about>

--

## 국내 발표자료
- [java 8 람다식 소개와 의미 고찰](http://www.slideshare.net/gyumee/java-8-lambda-35352385) (박성철)
- [자바8 람다 나머지 공개](http://www.slideshare.net/gyumee/8-37599530) (박성철)
- [Java8 람다식](http://www.slideshare.net/madvirus/8-35205661) (최범균)

## 책
- [가장 빨리 만나는 Java8](http://www.yes24.com/24/Goods/12991245?Acode=101)
- [폴리글랏 프로그램](http://www.yes24.com/24/goods/12204890)
- [자바 개발자를 위한 함수형 프로그래밍](http://www.hanbit.co.kr/ebook/look.html?isbn=9788979149678)
