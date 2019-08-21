notes on John Nesky's GDC talk: "50 Game Camera Mistakes"
(https://www.youtube.com/watch?v=C7307qRmlMI)

book he mentions: "real-time cameras" by Mark Haigh-Hutchinson

General comments:
the whole point of cameras is to focus attention on something other than themselves

cameras are most noticeable when they fail

embrace failure: "only through iteration that these problems get fixed"

in movies camera operators can deliberately plan out your shot in detail

everything in [a] game including the camera has to react to what the player is doing

in order for the the game camera to adapt [to the situation] ... we have to formalize the rules of a good camera design

there's basically three "game-atography" styles in video games. main difference is the distance between the camera and the avatar:

1.fixed angle third-person == the camera never rotates (camera that's basically
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
    "quaternions are often considered the correct way to represent orientations ...but that's not how players think players think in terms of Euler angles that means pitch and yaw they rotate sideways and they look up and down that's that's how players want to control with for example an analog stick or whatever it is they're using to control the camera so use the Euler angles to to represent the orientation of your camera"

4: Using a default camera distance that's likely to break line-of-sight.
    "the line of sight [is] ...like imaginary line between the camera and the avatar"
    "the entire camera code is basically designed around keeping this line of
    sight clear"

5: Allowing obstacles to break line-of-sight from the side.
    "the simplest way to fix the problem is to just detect by casting away from the avatars of the camera detect when the avatar has been included and just push the camera immediately closer to the to the avatar"

6: Pushing the camera away from an obstacle while the player is trying to swing the camera towards it.
    "30-degree rule means all camera cuts should change the angle by at least 30
    degrees ...failing to do that creates a jarring sense of motion"
    "much more pleasant when an obstacle is threatening to break line of sight to preemptively turn out of the way ...[to] detect these obstacles early with Ray casts that I call whiskers"
    "it is important to to always honor the players intent so if the player wants to turn the camera one way the camera must turn that direction"
    "as long as we know ahead of time that the obstacle is on the side that we're trying to avoid and the player is swinging towards it we can gradually interpolate the camera closer towards the avatar so that by the time the obstacles in the way we're already
    squeezing by it by getting closer letting the player push the camera inside an obstacle we still want to prevent breaking line of sight"

7: Letting the player push the camera inside an obstacle.
    "we can organize all the constraints [based around] separation of concerns"
    "player control it has the priority number"

8: Letting independent forces compete to push the camera.
    "the camera has to satisfy many constraints at a time and if constraints are allowed to compete with each other by applying forces in opposite directions they'll often cancel
    without satisfying either constraint but we can we can organize the constraints"(give them a hierarchy)
    "there are seven axes or degrees of freedom that matter to the camera there's a pitch and roll and the horizontal vertical and like forward backward offsets and field of view"

9: Excessively moving the camera to prevent unimportant items from breaking line-of-sight.

10: Letting the camera intersect narrow columns.
    "[not] breaking line of sight ...is important however there are cases where it would be more trouble than it's worth for the camera to try to avoid letting that happen"
    "we accomplish this by tagging obstacles based on whether or not they're allowed to break line of sight"

11: Interpreting a hill as a wall to be avoided.
    "it's okay to let ...narrow column[s] break line of sight however that's not okay to let the obstacle actually collide into the camera"
    "confusing to the player so this is where a more ordinary collision detection algorithms like a sphere collision check is helpful"
    "can check if the the camera itself is touching the column and if so just push
    it out until it's not touching anymore"
    "interpreting a hill as a wall... don't necessarily want to swing sideways away from the
    hills ...makes much more sense to rise the camera above the hill"
    "try checking the surface normal of the the slope to see if it's the kind of obstacle that you should be swinging away from or rising above"

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
17: Not cutting when the avatar passes through opaque objects.
18: Letting cuts remap directional controls.
19: Breaking the player's sense of direction.
20: Violating the 180 degree rule.
21: Focusing only on the avatar.
22: Relying on players to control the camera all the time.
23: Leaving the camera yaw alone while the player is running.
24: Making it hard to judge distances,
25: Looking straight ahead as the avatar approaches a cliff.
26. Keeping the camera level when the avatar is running on a slope.
27. Misusing the "Rule of thirds".
28. Using the same logic for ground and air motion.
29. Relying entirely on procedural camera behaviors.
30. Letting players make themselves lost and confused.
31. Rotating excessively to look at nearby targets.
32. Translating to look at distance targets.
33. Letting the avatar's own body occlude targets ahead.
34. Giving the player control over the camera, and then taking it away.
35. Immediately applying a camera hint after the player finished turning the camera to look at something.
36. Not letting experts explore.
37. Not providing inverted controls.
38. Responding to accidental controller input.
39. Using linear sensitivity.
40. Letting the camera pivot drift too far.
41. Using a too small field-of-view.
42. Rapidly shifting field-of-view.
43. Excessively shaking the camera.
44. Bouncing the camera with the avatar's walk cycle.
45. Translating or rotating up and down when the avatar jumps.
46. Rapidly transitioning to a new camera position.
47. Maintaining pitch speed until hitting the pitch limit.
48. Developing for the Oculus Rift as the primary camera method.
49. Testing with a narrow demographic.
50. Writing a general "constraint solver" that optimizes for the camera.


is shifting pitch distance and feel
the view independently so as I explained
as the camera is pitching up and down
the the distance needs to change and the
field that they also needs to change but
if these if these degrees of freedom are
not completely linked together then you
can create this weird effect where like
as you look up or down the the field of
view will change and that will affect
the size of everything on the screen
because field of view is zoom but then
the distance will also change it will
change in the opposite direction so you
might end up with the Avatar appearing
to expand in and shrink or shrink and
then expand so it's best to make sure
that the the the distance and the field
of view are both like derived from the
pitch so that they transition together
like gears however you do end up with
situations where other other factors
want to affect the distance the field of
view so in that case you can kind of
have like a base distance of field of
view there drive from pitch and then
other modifiers can like be multipliers
or offsets to be the base field of view
17 not cutting when the avatar passes
through opaque areas this is the case
where the line of sight is being
threatened from the front which is
actually super rare because the avatar
itself has collision and will prevent
obstacles from hitting the front of the
line of sight but there are cases for
exam
the sand fallen journey where an opaque
surface it can still be passed through
and in that case the only thing you can
really do to bring back line of sight is
to cut 18 is letting cuts remap
directional controls up on the on the
analog stick ring means go forward but
but but forward is determined by
whatever direction the camera is looking
in and when the camera suddenly rotates
to a different direction forward changes
and then up on the control stick changes
and players they physically can't
teleport their thumb when the camera
cuts they need a moment to adapt to the
new direction so this is a good
opportunity for either like a cutscene
or some kind of transition effect or or
just put the player in a situation where
they're not in danger of being attacked
or anything like that when when the cut
happens because players do need a moment
to adapt I'll totally alternatively you
could also try to interpolate the
forward direction during the cut I don't
know how well that works but some people
do it I've seen it happen 19 is breaking
the players sense of direction remapping
controls is a temporary problem because
the Fed can quickly adapt to whatever
the forward direction is on the camera
but cuts that change the camera angle
also create a longer-term loss of the
players sense of direction
I think players use primarily two
different techniques for navigating dead
reckoning which is keeping track of all
the changes in your position in
orientation so like if I rotate my body
180 degrees I know that North was is now
south or but a camera cut sorry the the
second technique is recognizing
landmarks so Dedrick can
in recognizing landmarks and camera cuts
they they make dead reckoning impossible
you can't tell all the time when a
camera cuts how much it has been rotated
so I don't know how much to rotate your
mental model of the map so this is a
crucial moment to make sure that the the
camera is showing the player easy either
recognizable landmarks or something to
help them understand where they are when
the camera is cutting so that they they
don't lose their sense of direction too
badly 20 is violating the 180 degree
rule this is a commonly known rule in
cinematography 180 degree rule means
don't cut the camera such that two
subjects like characters talking to each
other
swap sides that means like don't rotate
the camera behind them that kind of
thing but fortunately we don't actually
have to worry about that 180 degree rule
very often in games because as I've
explained cuts are dangerous and we
should avoid using them whenever
possible however there are situations
where 180 Google does apply especially
during cutscenes so like at the end of
the cut scene you should be showing me
the player what they they need to be
seeing to be able to navigate yeah 21 is
focusing only on the avatar so far I've
only been talking about the avatar but
yeah the player also needs to see other
stuff in the environment in order to
navigate for example the player needs to
be able to see the terrain immediately
around the the avatars feet to see
within out they're going to bump into a
wall or where they're going to fall off
a cliff and they also need to track a
keep track of things that are far away
like the mountain on the horizon and
journey or the partner they're traveling
with so number 22 is relying on players
to be controlling the camera at all
times for from any players controlling
an avatar and the camera at the same
time is a lot like patting your head and
rubbing your belly you can see a this is
Beyond Good and Evil and this player is
probably pretty good at this game but
they still end up like jerking the
camera around because this game doesn't
have any automatic guidance for the
camera so I recommend whenever possible
the camera should be doing its best to
look where the player wants to see in
advance so that the player doesn't have
to be controlling it 23 is leaving the
camera yah alone while the player is
running and the simplest way to help the
players see where they're going is to
show them where they're already going
just take the direction that the players
running in and map that to the the the
yaw of the camera so kind of like drift
behind me the avatar as they're running
however even this rule has a little bit
of nuance for example if the player is
running right into a wall it doesn't do
them any good to like show them a direct
view of the wall because there's nothing
beyond it so use use the players
velocity to figure out where they're
going rather than the direction of
pressing on the analog stick because
they may be pressing in like or we even
worse they may be running into an
invisible wall and you don't want to
taunt the player by showing them beyond
invisible you all you want to swing the
camera around the other direction to
show them where they should be going 24
is making it hard to judge distances I
love this game a lot but this is a
situation where the challenge is created
entirely by the camera kind of refusing
to align with the direction you're going
in the problem is that the screen is
2-dimensional and even if you've got
some kind of three-dimensional screen
depth perception is pretty weak players
are much better at judging distances
that align with the plane of the screen
and there's that so for example if you
have a top view
you're good at judging all horizontal
motion where's if you have a side view
you're good at judging vertical motion
and also horizontal motion on one axis
and in this case for this catwalk the
most important thing for the players to
know how far they are from either side
of the catwalk with that which means
that the cameras really should be like
pointing right down the catwalk so you
can see so you can like align that axis
with the side way sideways access of the
screen excuse me and 25 is looking
straight ahead as the Avatar approaches
a cliff when when player when people
walk towards a ledge they will look down
to see what's below and the counter
should be the same thing you can cast
more Ray's kind of kind of a downwards
ray in front of the avatar to see if
there's any drop in front of the avatar
and if you detect a drop the camera can
rotate its pitch to swing into a bird's
eye view and an additional benefit of
this in addition to just showing you
what's down there the camera is ready to
get out of the way if the player falls
off the cliff otherwise if the player if
the camera is level when you fall off a
cliff and the avatar Falls then the
camera wants to fall with it and will
crash into the clip itself but if it's
looking from a bird's eye view can just
like slip over the edge 26 is keeping
the camera level when the avatar is
running up or down a slope the same kind
of technique of for Clips applies the
slopes detect like what the the height
of the ground is in front of you and if
it's above or below you can look up and
down however some people might try to
just use like the shape of the Train
directly under the players feet but if
it's bumpy terrain that might give you
noisy information so you should be
really doing some kind of rate cast
ahead with the player to figure out the
like the average slope of the terrain 27
is misusing rule of thirds rule of
thirds is another well-known rule of
from cinematography and basically says
that pictures are more attractive when
the subjects are off-center usually
facing inwards this is a simple way to
improve the perceived quality of your
camera design but the the wrong way to
implement it is to to rotate the camera
in place like if you if you're trying to
implement rule of thirds by holding a
physical camera in your hands you'll
probably just rotate your hands a little
bit and you're pivoting in place but
again a game camera should never pivot
in place it should be rotating around
the avatar when it's doing the rotations
so instead in order to create an
off-center avatar you can just slide the
camera sideways the there's a problem
here however which is that players use
the center of the screen to aim their
analog stick if you have the player on
the off-center while they're running
they'll get confused about what
direction they're running in so in
journey we only use will of thirds when
the player is standing still when they
have to be running or when the players
analog stick we make the avatar drifts
back to the center
and I'm trying to clip some shadow of
the classes here because it's actually
an interesting counter example in this
game I think it's okay that the horse is
off-center as you're riding it because
in this game you're not using the analog
stick to to aim actually you're pressing
like the gas pedal to go forwards and
then you just use the analog stick to
press left and right to steer so in this
case the I think it's okay that the
player doesn't need to associate up on
the analog stick with the center of the
screen because you don't use the analog
stick to go forward
28 is using the same logic for ground
and air motion I've mostly been talking
about ground motions so far
but depending on the kind of game you
have there may be different kinds of
locomotion in the game and journey you
have running and flying shadowed for the
classes has as we said the the horse and
these different transportation modes
take different paths and trajectories
through space and the player needs to be
able to make different kinds of
judgments and in the case of flying in
journey what goes up always must come
down and it's important to always show
the player where they're going to land
so especially when you're running out of
juice in Journey have like a special
amount of or limited amount of flying
juice it's like at the very beginning
you might be flying upwards and the
camera will look up to show you where
you're going but as you're running out
of juice it's important to tilt the
camera downwards so that you're prepared
to land at any time to see where you're
going to land 29 is relying entirely on
procedural camera behaviors pretty much
all the camera behaviors I've described
so far are just automatic things that it
does to respond to the environment and
to the player but raycasting is is only
going to tell you the shape of the train
it doesn't tell you what's important
we as game designers need to decide this
this this other character in the game is
important or this this mountain is
important and the player needs to see
these things so in in our in our maps or
levels we we add scripted hints to tell
the camera in this context sensitive
sensitive situation the camera should be
pointing at this particular target
I think camera hints become even more
important in small closed environments
because dynamic cameras are pretty good
at open environments within weather
there's not very many
circles but in small closed environments
there's a lot of stuff for the camera to
watch out for and the more we can help
it out the better and here I have an
example from journey of a large tower
it's a very it's mostly complex re
convex shape which means there's like a
pretty well-defined center of the tower
and as long as the camera is on the
other side of the avatar from the center
of the tower then you can have a pretty
clear view of what's going on here so
this is a scripted hint in the game
that's that's not a general-purpose
behavior 30 is letting players make
themselves lost and confused players do
love to explore and we want to enable
them to do so but we also want to make
sure they don't get too lost and the
simplest solution employed by many games
is to put an error arrow or radar on the
screen I feel that these are a bit
heavy-handed we did not use them in
journey I prefer relying on subtle
camera tricks to suggest the direction
to to go for example as I as I said when
you're running up against a the boundary
of the world and journey the camera will
spin around to show the opposite
direction to point you back towards the
center or where you're supposed to go
another thing you might try doing is
detecting whether or not the player has
backtracked a significant distance and
if they have then spin the camera around
to point them forwards again 31 is
rotating to look at nearby targets when
you have a scripted hint that's telling
the counter to look at for example
another character or avatar we're
standing right next to you maybe your
first approach would be to make a camera
rotate to point at it but if you keep
doing that on the left you see Batman
Arkham Asylum and the camera just keeps
spinning around because these these two
things they're in close proximity to
keep can basically dancing around each
other and it's hard to keep up with that
kind of rotation
um but fortunately there's an
alternative approach here which is to
just pull the camera either to the side
or back to include more more in its view
in this journey for example what happens
is the camera pulls out right about now
to show both avatars without doing any
rotation and that that just is less
disorienting and more graceful 32 is
translating to look at distant targets
this is the opposite situation if you
have a nearby thing I should slide the
camera sideways or backwards to include
it but if you have a distant thing like
something on the horizon of the screen
or like the Sun for example no amount of
sliding the camera around would change
the position of the Sun on the screen so
the correct width thing to do in that
case is to rotate 33 is letting the
avatars own body occlude targets ahead
if you're rotating the camera to point
at something and the avatars in the
center of the screen there's a danger
the avatar itself will actually eclipse
what you're looking at you can point
either to the side or you can this would
be a good opportunity for rule of thirds
to slide to the science of the avatars
on the side of the screen but any case
you should just be be aware of this
situation often the the easiest thing to
do is to just make sure that the camera
is tilted down slightly so you can see
over the top of the app Tarzan 34 is
giving the player control over the
camera and then taking it away it's very
frustrating to give players control only
to take a big way again later so when
you're making these scripted camera
hints you should be aware of that or at
least enable the player to override the
hints whenever possible and if if you
have to make a hint that will override
player control make sure it's in a
situation where the camera is already
looking
at everything that the player needs to
see so that they won't feel the need to
control it 35 is immediately applying a
hint to a scripted hint to control the
camera after the player has finished
turning the camera to look at something
if the player is deliberately
controlling the camera they probably
want to be looking wherever they pointed
the camera and if you have a like a
scripted hint that's always on and
always trying to push the cam in a
certain direction as soon as you stop
overriding it it'll start it'll kick in
and again and turn the camera so the
there should be like a little delay
after controlling the camera where the
camera will continue to look where the
players wanted wants it to be pointed
and either after delay or after maybe
the player started moving again then you
can let the the can't kick in again 36
is not letting experts explore even if
the the player is technically still in
control of the camera and constantly
controlling it it can be frustrating
when overbearing hints are constantly
trying to get them to go in a different
direction
especially when experts are replaying in
game they're likely to want to go to new
places so after the hints have helped
the players establish their goals the
hint can be disengaged to allow the
player to vote to find their own drag
their own way to get there
37 is not providing inverted controls
this is a big problem I think pretty
much everyone in the game industry kind
of knows about this problem especially
first-person shooter developers but a
significant fraction of the game playing
population wants up to be down and the
other half or large fraction wants down
to be down and up to be up and they
probably will either refuse to play your
game or just complain a whole lot about
the game if they can't set the the
controls to be the way they want so make
sure you have an option somewhere in the
game
to invert the axes this is actually the
only option we have in journey four
controls 38 is responding to accidental
controller input in journey we made the
decision to support two different inputs
for controlling the camera there's the
the analog stick and there's also the
actual like a gyroscope or orientation
of the the ps2 controller and this is
partly left over from flour where you
control the game with the tilting the
controller honestly I think it makes it
made a lot more sense in flour because
it's the only thing you do in the game
where's in journey it's more of a a a
pattern your head and rub your belly
type of a problem and we end up in
journey with people often a tilting the
controller accidentally and accidentally
making the camera swing around there are
reasons why we we supported the the tilt
I think it is an intuitive control for
especially new players but I I think at
the very least we probably should have
had an option to disable it 39 is using
linear sensitivity especially on the
analog stick analog sticks don't have
much range often it doesn't matter much
honestly because players have a tendency
to push the stick all of the way you
kind of saw this from the Beyond Good
and Evil clip where the player kept
twitching the camera but I think it
would have been nicer to provide sort of
an intermediate or a very slow way a way
to control the camera very slowly for
fine-tuning the camera direction you can
do that by stretching the the input axis
from the the analog stick into a sort of
NS S curve that reduces the the neutral
positions for T is letting the camera
pivot drift too far so this means well
in order to smooth out
motion for example if you have an avatar
there's jerkily dashing back and forth
it's nice to smooth it out by letting
the the avatar drift from the center of
the screen but if you do that too much
if you let the camera be too lazy
then the avatar might actually run
outside the screen and that's a problem
so if you're using an elastic band type
thing to make the camera attracted to
the avatar make sure it has a limit to
to how far the avatar can get from the
center of the screen and you might
actually want the camera to look ahead
of the avatar for example in your shoes
island if if the player is running in a
certain direction up a little while the
camera will adjust by looking ahead of
the ocean
however problem 41 using a small field
of view another when the camera zooms in
on a tiny little thing then tiny little
motions become large motions and zooming
in exaggerate the speed of everything
and zooming out makes everything appear
to move much slower and here's a game
that is a I think nostalgic for a lot of
people I enjoyed the game quite a lot
but when I tried this is an example of a
game where when I try to show it to some
of my friends they are not able to play
the game because the camera just moves
too rapidly this is this is something I
care about a lot this is called
simulation sickness it's when people
play games and just from playing the
game and watching the screen they feel
sick and I wish that Yoshi's Island for
example or many other games would would
zoom out a bit more so you have a bit
more space to look around and the camera
won't have to to move as quickly to keep
up with the action on the screen
and on the subject of zooming out this
is also applicable to first-person
shooter games I don't have a clip but
you could easily put a first person
shooter clip on the screen this first
button shooters are actually probably
the most common trigger for simulation
sickness and the way that people usually
recommend that players deal with it is
by going into the configuration for the
game they're playing and expanding the
default field of view and as I explained
this has the effect of making everything
appear to move slower and I think this
is a good way to deal with a situation
if you're making a first-person shooter
especially for console where you don't
always have as much configuration
options include a way to expand the
field of view or just have expanded by
default so that you don't exclude
players who would otherwise be made sick
by your game 42 is rapidly shifting
field of view this is actually
increasing the problem in first-person
shooters when when aiming with a sniper
rifle or other type of weapon with a
scope the camera often zooms in rapidly
and also racing games do this when
you're boosting the camera will zoom out
to make it feel like you're going faster
and these kinds of things are also
triggers for simulation sickness so be
aware 43 is shaking the camera screen
shake is an effect that is commonly used
and all excuse me all kinds of video
games it's popular because it emphasizes
impacts it happens in the game game
world and in alongside vibration this is
used to make the game feel more real
some people say the screen shake is the
easiest way to make a game feel more
polished so it's clearly a very useful
tool for game developers but I want you
to be aware that not all players
experience shaking in the same way the
right amount of shake for you might be
too much shake for someone else so again
I urge you to have accessibility
settings for
are tuning down effects like screen
shake I have a clip from tower fall here
and I'm told the tower fall actually
does include a setting to turn off the
camera effects
so I'm cluding it here as a good example
44 bouncing the camera with the avatars
walk cycle this is another common one
especially in over the head games or
over the shoulder games this again is to
make the camera feel more real but
feeling real - you might not feel real -
someone else might feel nauseous to
someone else nauseating rather the the
goal is to sit the same as a screen
check again to make it feel more real
and the effect is similar camera bobbing
is just a longer slower shake and
equally likely to trigger simulation
sickness so you should be able to
disable that as well I have seen plenty
of games with an option to disable this
this is a common one do it please
translating or sliding up and down or
rotating up and down when the avatar is
jumping a lot of games both third-person
and side scrollers are have the camera
kind of locked onto the avatar one
mid-jump and that means like at the
moment when they jump in when they land
the camera has this abrupt start and
stop and that can be triggering as well
so this is another situation where we
know what goes up must come down
and we know that the camera the avatar
is probably not in danger of going off
the top of the screen so we can just let
the the camera stay in place as long as
they're just jumping up and down in
place and often what happens like in
mario games what happens is the camera
waits until the player has landed on a
higher surface before moving upwards in
journey we sort of a a rubber band
- just move without the the rapid
vertical motions that happen on the
screen 46 is rapidly transitioning to a
new camera position this this happens
often in games I think I think actually
some game developers take the rule of
not cutting a bit too far in games Zelda
games are the 3d Zelda games are
especially bad at this
I think these camera train transitions
might as well be camera cuts but because
there's smooth transitions but they're
really fast smooth transitions they
create a sense of motion that won't be
bare with a cut 47 is maintaining pitch
speed until hitting the limit one of the
downsides of using Euler angles is that
there's gimbal lock and in other
problems if you look straight up and
down so you pretty much never want to
have the allow the player to look
straight up and down you have one have
like some limit of how how high and down
there can look but I've seen a lot of
games will will make it so that if
you're pressing like up on the the
camera control stick it will rapidly
move the camera upwards up until hitting
the limit and they'll come to an abrupt
stop and it's much better to to
anticipate hitting the top-end - to slow
down as you approach the limit 48
this is probably a controversial thing
to put as a mistake but what I mean by
this is a that as games get more and
more lifelike and they're getting better
and better at simulating motion they're
also getting better and better at
triggering simulation signals because
simulation sickness is what happens when
there's a mismatch between what you see
the motion that you see in the motion
that you feel and I know that the oculus
rift developers are doing their best to
improve it and I'm sure they are
improving it but for the purposes of
maximizing your audience and not
excluding anybody I urge everyone to to
not make games exclusively for the
oculus rift make sure there's always
some other way to play the game because
some people I think will probably never
be able to play an oculus rift or at
least not in the next few years
49 testing with a narrow demographic
children especially are eager to point
out flaws and and other people who who
don't have a personal relationship with
you will be eager to criticize you but
you you may need to pry a bit with
testers because I know from experience
that some people who have simulation
sickness don't want to admit it it feels
like a weakness and you want to make
sure that they feel comfortable telling
you when there's a problem with your
game because I'm sure that none of you
want your customers to feel sick and 50
writing and general constraint solver
given the complexity of all of these
constraints that I've been describing
it's probably tempting to just come up
with a function that like evaluates how
effectively each of these constraints
has been satisfied and to just try to
optimize and pick the one angle that
satisfies them all the best now and let
the computer kind of figure that which
one that is which angle that is but I
believe this is actually almost never
the right approach because when you let
the computer do all the work it can be
very difficult to predict what the
results are going to be and if you can't
predict it and you can't really design
it you have to be able to iterate on the
design with changes that affect affect
the behavior in some unpredictable ways
so I think it's important to deeply
understand the relationship between the
different camera constraints so that
you're able to prioritize or otherwise
figure out how to satisfy all the
constraints or as best as possible see
so the devil is in the details
this solutions that work for journey
might not work for you so this lift list
can serve as criteria to help you
evaluate how well your solutions work
but it's not a replacement for doing
your own research so many of you will
have to do some of the same kind of
research and development that we have to
do on journey so good luck and first of
all I think I have two minutes for
questions one to two minutes for
questions I
anyone have any
hi so you mentioned a few options that
you think everybody should have you Bob
field of view camera shake do you think
it's worthwhile to combine all those
into a single option for people where
simulation sickness um that may be a
good idea although I think there's
there's some differences in how people
experience simulation sicknesses so some
things that are triggering to some
person some people might not be tricking
to someone else did you find that a lot
of people with simulation sickness were
very familiar with what precisely it was
that caused their simulation sickness
that is actually a good question that is
not always the case but if these options
can be surfaced you can let people know
about them so that they'll know what to
look for to figure it out okay thanks so
I know a bunch of us are probably gonna
go run and try to fix our third-person
cameras and while you mentioned that
like there's not a one-size-fits-all
there's definitely also some principles
it may be extrapolate yeah not to put
you on the spot any chance you're going
to like make a unity plugin or something
that's like a starting point that
already acknowledges some of the major
stuff so we can start from there instead
of like having to recreate some of these
foundations
I'm deeply concerning it it's a matter
of having time thank you now I'm you
good I think I'm a person
so I'm modifying field of view in order
to help with simulation sickness yeah
however in competitive first person
shooters field of view can also be
important to the players ability to
react to things on the edge of their
vision so it's actually the balanced
concern now if you have any thoughts or
experience with how to strike a
reasonable balance between those two
constraints well I remember reading
about in quake people a deliberately
expanding field of view for an advantage
so it sounds like expanded field of view
is an advantage for everyone just do it
alright you can contact me on Twitter
and I can probably find my way over to
the to the wrap up room somehow and
thank you
