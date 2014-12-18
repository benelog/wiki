# [spotify-engineering-culture](http://labs.spotify.com/2014/03/27/spotify-engineering-culture-part-1/)

<http://ucvox.files.wordpress.com/2012/11/113617905-scaling-agile-spotify-11.pdf>
<http://www.amazon.com/Tribal-Leadership-Leveraging-Thriving-Organization/dp/0061251321>


- Autonomous squads : Be autonomous, but don't suboptimize!
- Loosely coupled, tightly aligned squads
- Aligned Autonomy.
  - Autonomy vs Alignment의 2X2 그래프
      - Low autonomy, low alignment-> product becomes 프랑켄슈타인.
    - alignment Enables autonomy : The stronger aligment we have, the more autonomy we can afford to grant.
  - Leader's job : Copmmunicate what problem need to be solved. And why.
    - Squad's job : Collaborate with each other to find best solution
- Cross-pollination (교류, 타화수분) > Standards
  - 한 Squad에서 GIT이 좋아서 공유하면  점점 더 퍼지고 Defacto standard가 된다.
  - Healthy balace between consistency and Flexibility
- Internal opensource model
  - 다른 Squad의 코드도 고친다. Peer code review.
- Strong culture of mutual respect
  - little ego
- Focus on motivation
  > Hi everyone,
    > Our employee satisfaction survey says
    > 91% enjoy working here,
    > and 4% don't
    >
    > This is not satisfactory,
    > and we want to fix it.
    >
    > If you're one of those unhappy 4%,
    > please contat us.
    > We're here for your sake, and nothing else.
- Community > Structure
  - Tribe : Group of squad
  - Chapter : compentency area. QA, Agile coaching, web development.
      - Mentoring, Squad switching
  - Guild : Lightweight commounity of interest. Leadership, web development, Continous delivery
      - Mailing list, unconference,
- If you need to know extactly how is making decisions, you are in the wrong place.
- Encouraing a small frementquent release.
  - releasing is head -> release seldom. (악순환), releasing is easy ->  release often.
    - invest heavily on test automation , continous delivery infra structure.
    - Release should be routine, not drama.
  - Decoupled release를 할 수 있도록 변경한 사례
- Release trains + Feature toggles
  - unfininished features is hidden using a feature toggle.
    - Unmerged code hides technical probles, form of technical debts.
