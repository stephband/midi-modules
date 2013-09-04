# MIDI Modules


A specification for making interoperable midi modules.

## Modules

A module is a constructor. It returns an object representing an instance of a midi node.

    var midiFilter = MIDIFilter(options);

A node may have the following properties:

    {
        in: fn,
        out: fn
    }

It must have EITHER an <code>in</code> method OR an <code>out</code> method OR both.


### node.in

A method to call to pass MIDI messages into the node.

    midiGraph.in({ data: [144, 64, 80] });


### node.out

Method that takes a callback function and adds it to a queue.
The queue is called whenever this node generates a MIDI message.

    midiFilter.out(function(message) {
        // Do something with message
    });

Because <code>node.in</code> has the signature of a callback function, nodes can be connected thus:

    midiFilter.out( midiGraph.in );



## Messages

A midi message object contains the following properties:

    {
        data:         [n1, n2, n3]  // Required.
        receivedTime: float         // Optional.
        type:         'midimessage' // Optional.
        channel:      integer       // Optional.
        message:      string        // Optional.
    }

It is a subset of the DOM midimessage event.
This is deliberate, so that midimessage events can be passed directly to MIDI modules.
<code>channel</code> and <code>message</code> are optional, and are a result of processing <code>data</code>.
Because in effect this is a duplication of data, at the point <code>data</code> is processed it should be made immutable.
