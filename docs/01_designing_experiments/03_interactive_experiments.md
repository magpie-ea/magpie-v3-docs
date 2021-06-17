# Interactive experiments
Interactive experiments allow multiple participants to interact with each other.

## Creating a complex experiment
In order for interactivity to work, the magpie server needs to be aware of the rough structure of the experiment.
When creating the experiment on the server, we have to select `Complex experiment` to indicate that there is a dependency
between different realizations of the same experiment.

Complex experiments have three defining characteristics:

### Number of variants
The number of variants indicates how many participant groups there are. For example there might be two variants in an
experiment with an experimental group and a control group.

### Number of chains
A chain is a coherent unit inside one participant group. For example in an iterated narration experiment, all participants
of the same chain share the same story.

### Number of realizations
The number of realizations indicates how many participants are part of one chain.

## The experiment tri-tuple
These three characteristics indicate the total number of required participants, obtained by multiplying them. Each participant
can thus also be assigned a unique tri-tuple, indicating their variant, chain and realization ID.

## Building interactive experiments
Before participants can interact with other participants in their chain, the browser needs to connect to the magpie server
and wait for other participants.

This routine is conveniently encapsuled in the [`<ConnectInteractiveScreen>`](https://magpie-reference.netlify.app/#connectinteractivescreen)
component, which by default displays the following text:

> This screen sets up the socket connection for the interactive experiment and waits for other
> participants to join so that the number of required participants to start an interactive experiment is met. 

You can however replace it by entering your own text inside the component:

```html
<ConnectInteractiveScreen>
    Please wait for other participants...
</ConnectInteractiveScreen>
```

This screen will jump to the next screen in your experiment once a sufficient number of participants have joined the current chain.

## Communication
To allow participants to communicate, you can add the [`<Chat>`](https://magpie-reference.netlify.app/#chat) component
to your screens.

## Socket
In order to build more complex interactions, you can use [_magpie's socket](https://magpie-reference.netlify.app/#Socket) infrastructure.

This is a simplified Chat component that showcases how _magpie's socket works:

```html
<template>
  <div class="chat">
    <div ref="box" class="chat-box">
      <p
        v-for="(message, i) in messages"
        :key="i"
      >{{ message.message }}</p>
    </div>
    <div class="chat-input">
      <textarea
        ref="text"
        cols="50"
        placeholder="Type your message to the other participant here."
        @keydown.enter.prevent="send"
      ></textarea>
      <button @click.stop="send()">Send</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Chat',
  data() {
    return {
      messages: [],
    };
  },
  socket: {
    chat_message(payload) {
      this.messages.push(payload);
    }
  },
  methods: {
    send() {
      const message = this.$refs.text.value;
      this.$magpie.socket.broadcast('chat_message', {
        message,
        participantId: this.$magpie.socket.participantId,
        time: Date.now()
      });
    },
  }
};
</script>
```

Here, we define an array of messages in our component data. Whenever a socket event with the name `chat_message` arrives, we add
it to the array of messages, as defined by the `chat_message` function in the `socket` option of our component.

To send our own messages, we have a `send` method, which takes the value of the input field and broadcasts a `chat_message` event
to all other participants in our chain using [`$magpie.socket.broadcast`](https://magpie-reference.netlify.app/#Socket+broadcast).

The template then renders all chat messages and displays the input field for entering messages.

### Active participants
If you would like to know how many participants are currently active in the current screen, you can watch
[`$magpie.socket.active`](https://magpie-reference.netlify.app/#Socket+active), which is an array with the IDs of all participants currently
active in the current screen:

```html
<script>
export default {
  name: 'Chat',
  data() {
    return {
      messages: [],
    };
  },
  socket: {
    chat_message(payload) {
      this.messages.push(payload);
    }
  },
  computed: {
    activeParticipants() {
        return this.$magpie.socket.active
    }  
  },
  methods: {
    send() {
      const message = this.$refs.text.value;
      this.$magpie.socket.broadcast('chat_message', {
        message,
        participantId: this.$magpie.socket.participantId,
        time: Date.now()
      });
    },
  }
};
</script>
```
