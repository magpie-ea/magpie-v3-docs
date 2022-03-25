# Interactive and iterated experiments
Interactive experiments allow multiple participants to interact with each other, synchronously as well as asynchronously.

## Creating a complex experiment
In order for interactivity to work, the magpie server needs to be aware of the rough structure of the experiment.
When creating the experiment on the server, we have to select `Complex experiment` to indicate that there is a dependency
between different realizations of the same experiment.

Complex experiments have three defining characteristics:

* Variants
* Chains
* Generations
* Collaborators

  <img src="/images/getting_started/complex-experiments.png" />
An example experiment with 2 collaborators per generation, 3 generations per chain, and 2 groups per variant and 2 variants

### Collaborators
The number of collaborators indicates how many participants are in one session interacting live together. In an experiment where
pairs of people should work together, there would be 2 collaborators (see schema above).

### Generations
The number of generations indicates how many iterations the experiment should have. For example in an iterated narration
experiment (think Game of telephone) there would be one generation for each step in the retelling of the story (possibly with multiple collaborators each).

### Chains
A chain contains a line of generations. For example in an iterated narration experiment, every chain would develop their story independently.

### Variants
A variant contains multiple chains and defines the independent variables for those chains. In an iterated narration experiment
variants would be different initial stories, for example.

## The experiment tuple
These characteristics indicate the total number of required participants, obtained by multiplying them. Each participant
can thus also be assigned a unique tri-tuple, indicating their collaborator number, generation number and chain number and variant number.

## Building interactive experiments
Before participants can interact with other participants in their chain, the browser needs to connect to the magpie server
and wait for other participants.

This routine is conveniently encapsulated in the [ConnectInteractiveScreen](https://reference.magpie-experiments.org/#connectinteractivescreen)
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

## Interactivity
Interactivity is enabled by a socket connection from each participant to the server.

### Chat
To allow participants to communicate, you can add the [Chat](https://reference.magpie-experiments.org/#chat) component
to your screens, which uses this socket under the hood:

```html
<Experiment>
    <ConnectInteractiveScreen />

    <Screen>
        <p>Please discuss the text you have just read.</p>
        <Chat :data.sync="$magpie.measurements.chat" />
    </Screen>

</Experiment>
```

## Socket
In order to build more complex interactions, you can use [magpie's socket](https://reference.magpie-experiments.org/#Socket)
directly.

This is a simplified Chat component that showcases how magpie's socket works:

```html
<template>
  <div class="chat">
    <div ref="box" class="chat-box">
      <!-- Show all messages sent so far -->
      <p
        v-for="(message, i) in messages"
        :key="i"
      >{{ message.message }}</p>
    </div>
    <!-- Allow sending messages via Send button or via enter-->
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
    // for each chat_message socket event (including our own) we get a callback
    // and add the message to our stack of messages
    chat_message(payload) {
      this.messages.push(payload);
    }
  },
  methods: {
    // Get message text and send a chat_message event down the socket
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
to all other participants in our chain using [$magpie.socket.broadcast](https://reference.magpie-experiments.org/#Socket+broadcast).

The template then renders all chat messages and displays the input field for entering messages.

### Active participants
If you would like to know how many participants are currently active in the current screen, you can watch
[$magpie.socket.active](https://reference.magpie-experiments.org/#Socket+active), which is an array with the IDs of all participants currently
active in the current screen.

## Iterated experiments
Interactive experiments do not have to be synchronous.
Imagine for example an iterated narration experiment, where participants have to read and relay a story to each other
successively, similar to the game of Telephone.

### Waiting for the previous iteration
Similar to how we need for all participants to arrive in an interactive experiment, in an iterated experiment, we need to
wait for the results of previous iteration. This routine is encapsulated in [AwaitIteratedResultScreen](https://reference.magpie-experiments.org/#awaitIteratedresultscreen).

```html
<AwaitIteratedResultScreen>
    Please wait for other participants...
</AwaitIteratedResultScreen>
```

This screen will jump to the next screen in your experiment once the results of the previous iteration become available.

### Accessing previous results
Once the AwaitIteratedResultScreen has yielded to the next screen, the results from the previous iteration are available
in [$magpie.socket.lastIterationResults](https://reference.magpie-experiments.org/#Socket+lastIterationResults).

The iteration number is available in [$magpie.socket.generation](https://reference.magpie-experiments.org/#Socket+generation).

## Balancing randomization
Magpie's complex experiments can also be utilized to implement balanced randomization.

In this case, we use the *variant* to let the magpie backend sort participants into groups. *chains* should then be the
number of participants per variant, and *generations* can be `1` for this experiment (unless you want to create an iterated experiment).
After [ConnectInteractiveScreen](https://reference.magpie-experiments.org/#connectinteractivescreen) has
yielded to the next screen, we can access the assigned variant in `$magpie.socket`.
