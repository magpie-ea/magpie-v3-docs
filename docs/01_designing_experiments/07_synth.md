# Synthesizer
In addition to using pre-recorded audio files, magpie also provides a synthesizer for creating auditory stimuli.

The synthesizer is available via the [`<Synth>`](https://magpie-reference.netlify.app/#synth) component:

```html
<Synth :frequency="440" />
```

This will produce a 440 Hz tone via the participant's sound system as long as the component is being rendered
(i.e. as long as the current slide is visible).

You can also use pitch-octave notation to set the frequency:

```html
<Synth frequency="D3" />
<Synth frequency="F#3" />
<Synth frequency="A3" />
```

This will produce a D major triad.

Instead of letting the tone ring for as long as the component is being rendered, you can also specify a duration in seconds:

```html
<Synth frequency="D3" :duration="3" />
```

Similarly, the volume can be adjusted by specifying a normalized decibel value
(0 being the loudest, with the sound getting quieter as the value gets more negative)

```html
<Synth frequency="D3" :duration="3" :volume="-8" />
```

There are other controls available that you can experiment with in the reference: [`<Synth>`](https://magpie-reference.netlify.app/#synth)
