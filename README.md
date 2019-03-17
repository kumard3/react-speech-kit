# react-speech-kit 🎤&nbsp;&nbsp;&nbsp;&nbsp;[![CircleCI](https://circleci.com/gh/MikeyParton/react-speech-kit/tree/master.svg?style=shield)](https://circleci.com/gh/MikeyParton/react-speech-kit/tree/master) [![codecov](https://codecov.io/gh/MikeyParton/react-speech-kit/branch/master/graph/badge.svg)](https://codecov.io/gh/MikeyParton/react-speech-kit)
React components for in-browser Speech Recognition and Speech Synthesis.
[Demo here](https://mikeyparton.github.io/react-speech-kit/)

## Table of Contents

- [Install](#install)
- [Examples and Demo](#examples-and-demo)
- [SpeechSynthesis](#speechsynthesis)
  - [Props](#props)
  - [Render prop args](#render-prop-args)
- [SpeechRecognition](#speechrecognition)
  - [Props](#props-1)
  - [Render prop args](#render-prop-args-1)

## Install
```bash
yarn add react-speech-kit
```

## Examples and Demo
A full example can be found [here](https://mikeyparton.github.io/react-speech-kit/). The source code is in the [examples directory](https://github.com/MikeyParton/react-speech-kit/tree/master/examples/src).

## SpeechSynthesis
A react wrapper for the browser's [SpeechSynthesis API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis). It doesn't render anything itself, it just exposes the options and controls to the underlying SpeechSynthesis in the browser. You can use the children render prop to use these however you like.

### Props

#### onEnd
`function()`

Called when SpeechSynthesis finishes reading the text or is cancelled. It is called with no argumnents. Very useful for triggering a state change after something has been read.

#### children
`function({ Render prop args })` _(required)_

Render prop function to consume the arguments from SpeechSynthesis and render your own controls. It is called with an object containing render prop args.

### Render prop args

#### speak
`function({ text: string, voice: SpeechSynthesisVoice })`

Call to make the browser read some text. You can change the voice by passing an available SpeechSynthesisVoice (provided in the voices prop). Note that some browsers require a direct user action to start using SpeechSynthesis. To make sure it works, it is recommended that you call speak for the first time in a click event handler.

#### cancel
`function()`

Call to make SpeechSynthesis stop reading.

#### speaking
`boolean`

True when SpeechSynthesis is actively speaking.

#### supported
`boolean`

Will be true if the browser supports SpeechSynthesis. Keep this in mind and use this as a guard before rendering any control that allow a user to call speak.

#### voices
`[SpeechSynthesisVoice]`

An array of available voices which can be passed to the speak function. An example SpeechSynthesisVoice voice has the following properties.
```
{
  default: true
  lang: "en-AU"
  localService: true
  name: "Karen"
  voiceURI: "Karen"
}
```
In some browsers voices load asynchronously. In these cases, the array will be empty until they are available. To manage the selected voice, you should store the index of the voice you would like to use in state.

## SpeechRecognition
A react wrapper for the browser's [SpeechRecognition API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition). Again, it doesn't render anything itself, it just exposes the options and controls to the underlying SpeechRecognition in the browser. You can use the children render prop to use these however you like.

### Props

#### onEnd
`function()`

Called when SpeechRecognition stops listening.

#### onResult
`function(string)`

Called when SpeechRecognition has a result. It is called with a string containing a transcript of the recognized speech.

#### children
`function({ Render prop args })` _(required)_

Render prop function to consume the arguments from SpeechRecognition and render your own controls. It is called with an object containing the following values.

### Render prop args

#### listen
`function({ interimResults: boolean, lang: string })`

Call to make the browser start listening for input. Every time it processes a result, it will forward a transcript to the provided onResult function. You can modify behavior by passing the following arguments:

- **lang**  
   `string`  
   The language the SpeechRecognition will try to interpret the input in. Use the languageCode from this list of languages that Chrome supports ([here](https://cloud.google.com/speech-to-text/docs/languages)) e.g: "en-AU". If not specified, this defaults to the HTML lang attribute value, or the user agent's language setting if that isn't set either.

- **interimResults**  
   `boolean` _(default: true)_   
   SpeechRecognition can provide realtime results as it's trying to figure out the best match for the input. Set to false if you only want the final result.

#### stop
`function()`

Call to make SpeechRecognition stop listening. This will call the provided onEnd function as well.

#### listening
`boolean`

True when SpeechRecognition is actively listening.

#### supported
`boolean`

Will be true if the browser supports SpeechRecognition. Keep this in mind and use this as a guard before rendering any control that allow a user to call listen.
