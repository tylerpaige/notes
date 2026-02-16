---
Author: Dave Gurnell
Full Title: Compositional Music Composition - Underscore
Category: #articles
Publication date: 2015-03-04
URL: http://underscore.io/blog/posts/2015/03/05/compositional-music-composition.html
Summary: Noel recently wrote about Doodle, the compositional drawing library featured in Creative Scala and our new studio-format Essential Scala.
---

![rw-book-cover](http://underscore.io/images/twitter-card-icon.png)

# Notes

[[Dave Gurnell - Compositional Music Composition - Underscore]]

***

Noel [recently wrote](https://underscore.io/blog/posts/2015/01/29/rethinking-online-training.html) about [Doodle](https://github.com/underscoreio/doodle), the compositional drawing library featured in [Creative Scala](https://underscore.io/training/courses/creative-scala) and our new studio-format [Essential Scala](https://underscore.io/training/courses/essential-scala). Today I want to introduce you to another library called [Compose](https://github.com/underscoreio/compose). This new library, which will be featured in future courses, applies the same functional programming principles to music.

I’ll be [speaking](http://asyncjs.com/creative-functional-programming/) about Doodle and Compose at [Async](http://asyncjs.com/) in Brighton on March 19th. All welcome—come and say hi!

Doodle and Compose are both designed in a classicly functional manner. The user builds a representation of the desired output using a set of primitive objects and combinators. The library then interprets the representation to produce the final result. Although we have used this idea of combinator libraries for media applications, the same ideas apply to business applications.

* User code is simple and declarative, describing only the expected output and ignoring implementation-specific details.
* The intermediate representation, if defined correctly, allows the user to easily change aspects of the output: relocating an image to a different part of the screen, or playing a song back at a different tempo.
* We can provide different interpreters for different situations without changing any user code. For example, Doodle has an interpreter for Swing/Java2D and an interpreter for HTML5/Canvas.
* Finally, it is possible to clone and re-use parts of images and songs within other images and songs. This is the fundamental nature of a *composable* representation.

#### Representing Music as Code

Doodle is based on simple geometric primitives such as squares, circles, and triangles. Its composition operations are spatial functions such as `beside`, `above`, and `below`. Let’s look at the language Compose uses to represent music and how it allows us to build complex songs from simple parts.

#### Primitives: Notes, Rests, Pitch, and Duration

The primitives in a musical score are *notes* and *rests*. Notes have a *pitch* and *duration* and rests simply have a *duration*.

Piano keys are a convenient represenation of pitch: `C4` is middle C, `D4` the next white note, `Cs4` the C sharp between the two, and so on.

Durations are measured in *beats*. There are typically four beats a bar but many notes are much shorter than that. Compose provides representations for whole, half, quarter, eighth, sixteenth, and 32nd beats, and combinators to produce [“dotted” variants](https://en.wikipedia.org/wiki/Dotted_note).

![](https://underscore.io/images/blog/2015-03-05-compositional-music-composition-pitches.jpg)[Photograph from the Leeds Piano Competition, CC-BY.](https://www.flickr.com/photos/124497826@N08/14121388525)
The end result is a DSL for producing notes with any cross section of pitch and duration:

```
import Note._

C4.e        // C4,  eighth beat duration
Fs4.h       // F#4, half beat duration
D5.w.dotted // D5,  dotted whole beat duration

```

Note that we are omitting another important component of music – *dynamics*. Without changes in volume, our musical compositions will sound artificial. However, we’ve opted to keep things simple for now and save this for a future addition to the library.

#### Composition

Now we have our primitive building blocks, let’s think about how we can combine them to create musical scores. We can combine notes in a *sequence* (played one after the other) and *parallel* (played at the same time). Compose uses the `+` operator for sequential composition and the `|` operator for parallel composition[1](https://underscore.io/blog/posts/2015/03/05/compositional-music-composition.html#fn:monoid):

```
import Note._

// A C major chord:
C4.e | E4.e | G4.e

// A C major scale:
C4.e + D4.e + E4.e + F4.e + G4.e + A4.e + B4.e

```

And, of course, sequential and parallel forms compose nicely. Here’s a simple chord progression:

```
val Cmaj   = C3.q | E3.q | G3.q
val Fmaj   = C3.q | F3.q | A3.q
val Gmaj   = D3.q | G3.q | B3.q
val chords = Cmaj + Fmaj + Gmaj + Fmaj + Cmaj

```

Behind the scenes Compose builds up a representation of the music using `Score` objects. There are `Score` wrappers for notes, rests, sequences, and parallel combinations:

```
sealed trait Score {
  def +(that: Score) = SeqScore(this, that)
  def |(that: Score) = ParScore(this, that)
}
case class NoteScore(note: Note, duration: Duration) extends Score
case class RestScore(duration: Duration) extends Score
case class SeqScore(a: Score, b: Score) extends Score
case class ParScore(a: Score, b: Score) extends Score

```

Ignoring dynamics and volume, we can represent any composition using `Scores`.

#### Playback

To play songs back, we have to think of them as programs to be compiled and interpreted. You can review the code Compose uses for this on [Github](https://github.com/underscoreio/compose). The essential idea is to compile the score into a sequence of `Commands` that can be interpreted directly:

```
sealed trait Command
case class NoteOn(channel: Int, freq: Double) extends Command
case class NoteOff(channel: Int) extends Command
case class Wait(millis: Long) extends Command

```

The player, which uses the [ScalaCollider](http://www.sciss.de/scalaCollider/) library to communicate with [SuperCollider](http://audiosynth.com/), allocates a fixed number of *channels* to play back sounds. If we have 4 channels it means we can play 4 samples at the same time. The `NoteOn` and `NoteOff` commands affect a specified channel and the `Wait` command delays for a specified time period.

`NoteOn` and `NoteOff` commands with too high a `channel` are ignored during playback, allowing for graceful degradation if the song we’re playing contains more simultaneous notes than we have channels. The compiler also has a notion of tempo, allowing us to play the same score quickly or slowly depending on user preferences.

#### Summary

*Compose* demonstrates how classical functional programming principles can be applied to music. We model a problem domain using *primitives* such as notes and rests, and *combine* them using parallel and sequential operators. Finally, we *interpret* the resulting score to produce a useful output.

Functional library design allows library users to concentrate on the components and relationships in their compositions without concerning themselves with implementation details such as then number of channels available for playback. Songs can be combined and transformed to create new songs with different patterns or in different keys.

You might validly argue that, while the DSL used in Compose is expressive enough to store music, it isn’t particuarly readable as regular sheet music. One final feature of a functional library design is the ability to create new DSLs on top of the existing representations and interpreters. More on this in a future post!

1. `+` and `|` are actually examples of [monoids](http://eed3si9n.com/learning-scalaz/Monoid.html) on `Score`. In each case the corresponding `zero` is an empty `Score`. [↩](https://underscore.io/blog/posts/2015/03/05/compositional-music-composition.html#fnref:monoid)
