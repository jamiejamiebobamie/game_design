notes on John Nesky's GDC talk: "50 Game Camera Mistakes"
(https://www.youtube.com/watch?v=C7307qRmlMI)

book he mentions: "real-time cameras" by Mark Haigh-Hutchinson

General comments:
the whole point of cameras is to focus attention on something other than themselves

cameras are most noticeable when they fail

embrace failure: "only through iteration that these problems get fixed"

in movies, camera operators can deliberately plan out your shot in detail

everything in [a] game including the camera has to react to what the player is doing

in order for the the game camera to adapt [to the situation] ... we have to formalize the rules of a good camera design

there's basically three "game-atography" styles in video games. main difference is the distance between the camera and the avatar:

1.fixed-angle third-person == the camera never rotates (camera that's basically
infinitely far away. pretty much have to tear down everything between the avatar and the cameras.)

2.dynamic angle third-person
    "on this weird sliding scale between close and far camera distances with kind of all the downsides of both"
    "look in any direction and explore on their own"
    "clear view of the avatars body"
    "a camera that physically fits inside the world"
    "have to worry about all of these collision detection problems"
    "keep the space between the camera and the avatar empty"
    "design constraints leave us with a whole lot of problems"
    "the hardest style of camera to design for a video game"

3.first-person (a distance of zero the camera is literally inside the skull of the avatar)

50 Game Camera Mistakes:

1: Using a dynamic camera when another approach would work.
    "games developed under tight constraints ...recommend picking an easier camera style"

2: Designing levels and camera behaviors that don't match.
    "level design and camera design are closely related disciplines and they need to cooperate"
    "in third person games ...the focus of the tension is really the avatar not the camera"

3: Using global coordinates or quaternions to persist camera state.
    "whenever the camera rotates it should always be pivoting around the avatar like a moon orbiting around"
    "not important to store the global XYZ coordinates of the camera"
    "can always just derive those as an offset from the avatar"
    "quaternions are often considered the correct way to represent orientations ...but that's not how players think players think in terms of Euler angles that means pitch and yaw they rotate sideways and they look up and down that's how players want to control with for example an analog stick or whatever it is they're using to control the camera so use the Euler angles to represent the orientation of your camera"

4: Using a default camera distance that's likely to break line-of-sight.
    "the line of sight [is] ...like imaginary line between the camera and the avatar"
    "the entire camera code is basically designed around keeping this line of
    sight clear"

5: Allowing obstacles to break line-of-sight from the side.
    "the simplest way to fix the problem is to just detect by casting away from the avatars of the camera detect when the avatar has been included and just push the camera immediately closer to the avatar"

6: Pushing the camera away from an obstacle while the player is trying to swing the camera towards it.
    "30-degree rule means all camera cuts should change the angle by at least 30
    degrees ...failing to do that creates a jarring sense of motion"
    "much more pleasant when an obstacle is threatening to break line of sight to preemptively turn out of the way ...[to] detect these obstacles early with ray casts that I call whiskers"
    "it is important to to always honor the players intent so if the player wants to turn the camera one way the camera must turn that direction"
    "as long as we know ahead of time that the obstacle is on the side that we're trying to avoid and the player is swinging towards it we can gradually interpolate the camera closer towards the avatar so that by the time the obstacles in the way we're already squeezing by it by getting closer letting the player push the camera inside an obstacle we still want to prevent breaking line of sight"

7: Letting the player push the camera inside an obstacle.
    "we can organize all the constraints [based around] separation of concerns"
    "player control it has the priority number"

8: Letting independent forces compete to push the camera.
    "the camera has to satisfy many constraints at a time and if constraints are allowed to compete with each other by applying forces in opposite directions they'll often cancel [each other] without satisfying either constraint but we can we can organize the constraints"(give them a hierarchy)
    "there are seven axes or degrees of freedom that matter to the camera there's a pitch and roll and the horizontal vertical and like forward backward offsets and field of view"

9: Excessively moving the camera to prevent unimportant items from breaking line-of-sight.

10: Letting the camera intersect narrow columns.
    "[not] breaking line of sight ...is important however there are cases where it would be more trouble than it's worth for the camera to try to avoid letting that happen"
    "we accomplish this by tagging obstacles based on whether or not they're allowed to break line of sight"
    "it's okay to let ...narrow column[s] break line of sight however that's not okay to let the obstacle actually collide into the camera"
    "confusing to the player so this is where a more ordinary collision detection algorithms like a sphere collision check is helpful"
    "can check if the the camera itself is touching the column and if so just push
    it out until it's not touching anymore"

11: Interpreting a hill as a wall to be avoided.
    "interpreting a hill as a wall... don't necessarily want to swing sideways away from the hills ...makes much more sense to rise the camera above the hill"
    "try checking the surface normal of the slope to see if it's the kind of obstacle that you should be swinging away from or rising above"

12: Swinging the camera sideways when occluders come from behind.
    "line of sight can be broken from many directions"
    "avatar run towards the camera and the camera is backing up it can be backed up
    into a corner"
    "you should use a ray cast like immediately behind the camera to detect if its being
    back[ed] into a corner so just pull closer without swinging sideways"

13: Letting the camera's near-clipping-plane intersect the avatar.
    "all virtual cameras have what's called a near clipping plane and anything closer to that gets cut off and leaves a big gaping hole in the object"
    "there needs to be enough space between the avatar and any adjacent walls for the camera to fit it's near clipping plane through the space"
    "can do that is by making sure that the the avatars collision radius is wider than the actual avatar so that they'll never actually like press up right up against the wall"

14: Using the same camera distance for all angles.
    "I'm talking about up and down tilting if you're if you're looking up that means the camera has to swing downwards to keep the avatar in view"
    "it can feel a little bit claustrophobic"
    "I recommend pulling the camera out a little bit when you're looking downwards"

15: Using the same field-of-view for worm's eye angles and standard angles.
    "as humans we use our peripheral vision to see the most or all of the sky we can see almost 180 degrees to our sides"
    "in order to create the same feeling when the game is looking at the sky [the camera] should expand its field of view as its transitioning to a bird's-eye view"

16: Shifting pitch, distance, and field-of-view independently.
    "make sure that the [camera] distance [from the player] and the field of view are both derived from the pitch so that they transition together like gears"

17: Not cutting when the avatar passes through opaque objects.
    "the line of sight is being threatened from the front ...super rare because the avatar ...has collision and will prevent obstacles from hitting the front of the line of sight but there are cases for example the sand falling [in] Journey where an opaque surface ...can still be passed through ...the only thing you can really do to bring back line of sight is to cut"

18: Letting cuts remap directional controls.
    don't remap the directional controls after a cut
    "when the camera suddenly rotates to a different direction forward changes and then up on the control stick changes and players they physically can't teleport their thumb when the camera cuts"
    sometimes unavoidable.
    you can try interpolating from one forward to direction to a new one, but speaker has not tried this.

19: Breaking the player's sense of direction.
    "cuts that change the camera angle ...create a longer-term loss of the player's sense of direction"
    "make sure that the camera is showing ...recognizable landmarks or something to help them understand where they are when the camera is cutting so that they they don't lose their sense of direction too badly."

20: Violating the 180 degree rule.
    "180 degree rule means don't cut the camera such that two subjects like characters talking to each other swap sides [of the screen] ...don't actually have to worry about that 180 degree rule very often in games however there are situations where 180 rule does apply especially during cutscenes"

21: Focusing only on the avatar.
    "the player needs to be able to see the terrain immediately around the the avatar's feet to see whether they're going to bump into a wall or where they're going to fall off a cliff and they also need to keep track of things that are far away like the mountain on the horizon and [other players and enemies]"

22: Relying on players to control the camera all the time.
    "for [some] ...players controlling an avatar and the camera at the same time is a lot like patting your head and rubbing your belly [at the same time]"
    "whenever possible the camera should be doing its best to look where the player wants to see in advance so that the player doesn't have to be controlling it"

23: Leaving the camera yaw alone while the player is running.
    "take the direction that the players running in and map that to the yaw of the camera so kind of like drift behind me the avatar as they're running"
    "however ...if the player is running right into a wall it doesn't do them any good to like show them a direct view of the wall"

24: Making it hard to judge distances.
    "depth perception is pretty weak [in video games]"
    "players are much better at judging distances that align with the plane of the screen"
    "a top view == good at judging all horizontal motion"
    "a side view == good at judging vertical motion and also horizontal motion on one axis"
    "in [the] case [of a] catwalk the most important thing for the players to know [is] how far they are from either side of the catwalk ...which means that the cameras really should be like pointing right down the catwalk so you can like align that axis
    with the sideways axis of the screen"

25: Looking straight ahead as the avatar approaches a cliff.
    "when people walk towards a ledge they will look down
    to see what's below"
    "cast a ray downwards in front of the avatar to see if there's any drop in front of the avatar ..if you detect a drop the camera can rotate its pitch to swing into a bird's eye view ...an additional benefit of this [is] the camera is ready to get out of the way if the player falls off the cliff otherwise"

26. Keeping the camera level when the avatar is running on a slope.
    "figure out what the height of the ground is in front of you and if it's above or below you can look up or down ...ray cast ahead of the player to figure out the average slope of the terrain [is]"
    if terrain is bumpy, it will vary, which will create noise.

27. Misusing the "Rule of thirds".
    "rule of thirds basically says that pictures are more attractive when the subjects are off-center usually facing inwards this is a simple way to improve the perceived quality of your camera design but the the wrong way to implement it is to to rotate the camera in"
    "a game camera should never pivot in place it should be rotating around the avatar"
    "in order to create an off-center avatar you can just slide the camera sideways"
    "players use the center of the screen to aim their analog stick so only use the rule of thirds when the player is standing still when the avatar is running or when the players use the analog stick we make the camera drift back to the center"

28. Using the same logic for ground and air motion.
    "different kinds of locomotion in the game"
    "journey [has] ...running and flying ...[some games have a] horse"
    "these different transportation modes take different paths and trajectories through space"
    "the player ...make[s] different kinds of judgments"
    "flying ...it's important to always show the player where they're going to land ...especially when you're running out of juice ...important to tilt the camera downwards so that you're prepared to land"

29. Relying entirely on procedural camera behaviors.
    "raycasting is is only going to tell you the shape of the terrain"
    "game designers need to decide ...this mountain is important ...add scripted hints to tell the camera in this context sensitive sensitive situation the camera should be
    pointing at this particular target"
    "camera hints become even more important in small closed environments"
    "dynamic cameras are pretty good at open environments"
    "in small closed environments there's a lot of stuff for the camera to watch out for and the more we can help it out the better"

30. Letting players make themselves lost and confused.
    "players do love to explore ...enable them to do so"
    "but ...make sure they don't get too lost"
    "I prefer relying on subtle camera tricks to suggest the direction to to go"
    "the camera point[s] you back towards ...where you're supposed to go"

31. Rotating excessively to look at nearby targets.
    "just pull the camera either to the side or back to include more more in its view [if the camera is dancing around between two targets in close proximity]"

32. Translating to look at distance targets.
    "if you have a nearby thing I should slide the camera sideways or backwards to include it but if you have a distant thing like something on the horizon of the screen or like the Sun for example no amount of sliding the camera around would change the position of the Sun on the screen so the correct ...thing to do in that case is to rotate"

33. Letting the avatar's own body occlude targets ahead.
    "if you're rotating the camera to point at something and the avatars in the center of the screen there's a danger the avatar itself will actually eclipse what you're looking at"
    "can point either to the side or ...good opportunity for rule of thirds to slide to the side of the avatars on the side"
    "often the the easiest thing to do is to just make sure that the camera is tilted down slightly so you can see over the top of the avatar"

34. Giving the player control over the camera, and then taking it away.

35. Immediately applying a camera hint after the player finished turning the camera to look at something.
    "if the player is deliberately controlling the camera they probably want to be looking wherever they pointed the camera"
    "there should be like a little delay after controlling the camera where the camera will continue to look where the players wanted wants it to be pointed"
    "either after [a] delay or after maybe the player start[s] moving again then you can let the hint kick in again"

36. Not letting experts explore.
    "after the hints have helped the players establish their goals the hint can be disengaged to allow the player to find their own their own way"

37. Not providing inverted controls.

38. Responding to accidental controller input.

39. Using linear sensitivity.
    "analog sticks don't have much range"
    "players have a tendency to push the stick all of the way"
    "nicer to provide sort of an intermediate or a very slow way to control the camera very slowly"

40. Letting the camera pivot drift too far.
    "for fine-tuning the camera direction you can do that by stretching the the input axis from the the analog stick into a sort of NS S(?) curve that reduces the the neutral
    positions"
    "if you're using an elastic band type thing to make the camera attracted to
    the avatar make sure it has a limit to to how far the avatar can get from the center of the screen and you might actually want the camera to look ahead of the avatar"

41. Using a too small field-of-view.
    "simulation sickness [is] when people play games and just from playing the game and watching the screen they feel sick"
    "many ...games would would zoom out a bit more so you have a bit more space to look around and the camera won't have to to move as quickly"
    "this is also applicable to first-person shooter games [the fix is] expanding the default field of view and as I explained this has the effect of making everything appear to move slower and I think this is a good way to deal with a situation if you're making a first-person shooter"

42. Rapidly shifting field-of-view.
    "also [a] trigger for simulation sickness so be aware"

43. Excessively shaking the camera.
    "is shaking the camera screen is popular because it emphasizes impacts ...alongside vibration ...used to make the game feel more real"
    "have accessibility settings for ...tuning down effects like screen shake"

44. Bouncing the camera with the avatar's walk cycle.
    "common ...in over the head games or over the shoulder games"
    "make[s] the camera feel more real"
    "but someone else might feel nauseous"
    "should be able to disable that as well"

45. Translating or rotating up and down when the avatar jumps.
    "[fix:] sort of a a rubber band - just move without the rapid vertical motions"

46. Rapidly transitioning to a new camera position.

47. Maintaining pitch speed until hitting the pitch limit.
    "one of the downsides of using Euler angles is that there's gimbal lock"
    "better to anticipate hitting the top-end - to slow down as you approach the limit"

48. Developing for the Oculus Rift as the primary camera method.
    "simulation sickness is what happens when there's a mismatch between what you see the motion that you see in the motion that you feel"

49. Testing with a narrow demographic.

50. Writing a general "constraint solver" that optimizes for the camera.
    "it's probably tempting to just come up with a function that like evaluates how effectively each of these constraints has been satisfied and to just try to optimize and pick the one angle that satisfies them all the best now and let the computer kind of figure [it out]"
    "almost never the right approach"
    "when you let the computer do all the work it can be very difficult to predict what the results are going to be and if you can't predict it and you can't really design it"
    "it's important to deeply understand the relationship between the different camera constraints so that you're able to prioritize or otherwise figure out how to satisfy all the constraints"
