You're encouraged to use this client along with any Haxe Game Engine, such as: [OpenFL](https://www.openfl.org/), [Kha](http://kha.tech/), [HaxeFlixel](http://haxeflixel.com/), [Heaps](https://heaps.io/), [HaxePunk](http://haxepunk.com/), etc.

## Installation

Install the latest version from the git repository:

```
haxelib git colyseus https://github.com/colyseus/colyseus-hx.git
haxelib git haxe-ws https://github.com/colyseus/haxe-ws.git
```

## Usage

### Connecting to server:

```haxe
import io.colyseus.Client;
import io.colyseus.Room;

var client = new Client('ws://localhost:2567');
```

### Joining to a room:

You need to provide your [root Schema state](/state/schema/) as third argument when joining a room.

```haxe
var room = client.join("room_name", [], State);
room.onJoin += function() {
    trace(client.id + " joined " + room.name);

    room.state.entities.onAdd = function(entity, key) {
        trace("entity added at " + key + " => " + entity);

        entity.onChange = function (changes) {
            trace("entity changes => " + changes);
        }
    }

    room.state.entities.onChange = function(entity, key) {
        trace("entity changed at " + key + " => " + entity);
    }

    room.state.entities.onRemove = function(entity, key) {
        trace("entity removed at " + key + " => " + entity);
    }
}
```

### Other room events

Room state has been updated:

```haxe
room.onStateChange += function(state) {
  // full new state avaialble on 'state' variable
}
```

Message broadcasted from server or directly to this client:

```haxe
room.onMessage += function (message) {
  trace(client.id + " received on " + room.name + ": " + message);
}
```

Server error occurred:

```haxe
room.onError += function() {
  trace(client.id + " couldn't join " + room.name);
}
```

The client left the room:

```haxe
room.onLeave += function() {
  trace(client.id + " left " + room.name);
}
```

## Running the demo project

The [`example`](https://github.com/colyseus/colyseus-hx/blob/master/example/openfl) project can be compiled to `html5`, `neko`, `cpp`, `ios`, etc.

It uses the `state_handler` room from the [colyseus-examples](https://github.com/colyseus/colyseus-examples) project, which you can find [here](https://github.com/colyseus/colyseus-examples/blob/master/rooms/02-state-handler.ts).

### Compiling the demo project to `html5`

```
git clone https://github.com/colyseus/colyseus-hx.git
cd colyseus-hx/example/openfl
lime build project.xml html5
```

You can see the demo project [live here](http://colyseus.io/colyseus-hx/).


## `ios` target caveats

You may need to manually apply this patch in order to compile for iOS: [HaxeFoundation/hxcpp@5f63d23](https://github.com/HaxeFoundation/hxcpp/commit/5f63d23768988ba2a4d4488843afab70d279a593)

> More info:
> http://community.openfl.org/t/solved-system-not-available-on-ios-with-xcode-9-0/9683?source_topic_id=10046
