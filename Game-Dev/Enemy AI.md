In the enemy AI, the AI enemies must not collide with each other when they go in opposite directions, this is known as RVO (Reciprocal Velocity Obstacles), i.e. they become obstacles for each other that have reciprocal velocities compared to each other, so they end up stopping each other completely for a brief while before they brush past each other weirdly.

So there is a method to ensure that this does not happen, the name of that method is Detour Crowd Avoidance. In this, each of the enemies is aware of a few things already, namely -> aware of other agents, then alters its velocity and heading to follow a new path around them and respects the Nav mesh boundaries.

![[Pasted image 20241006101156.png]]
![[Pasted image 20241006101211.png]]
![[Pasted image 20241006101228.png]]
![[Pasted image 20241006101255.png]]
![[Pasted image 20241006101318.png]]
![[Pasted image 20241006101347.png]]
