# MOS Dummy Server

A dummy MOS (Media Object Server) server for testing MOS clients with failover capabilities. This server implements the MOS protocol and allows you to simulate connection issues and manage rundowns through JSON files.

## Features

- MOS protocol support Profiles 0, 1, 2, 3, and 4
- JSON-based rundown downs in the `rundowns` directory
- Hot reloading of rundowns from the filesystem
- Simulation of server outages for failover testing
- Command-line interface for manual testing

## Installation
Install dependencies:

```bash
yarn install
```

### Development mode
To run in development mode with automatic reloading:

```bash
yarn dev
```

you can add a --secondary flag to run a secondary server on a different port:

```bash
yarn dev --secondary
```

Or with file watching:

```bash
yarn run watch
```

### Run on same machine as client:
To run on same machine as client, remember to set the clients acceptsConnections to false (here shown in Sofie):

```bash
mos: {
		self: {
			debug: debug,
			// mosID: 'sofie.tv.automation',
			mosID: 'N/A', // set by Core
			acceptsConnections: false, // default:true
			// accepsConnectionsFrom: ['127.0.0.1'],
			profiles: {
				'0': true,
				'1': true,
				'2': true,
				'3': false,
				'4': false,
				'5': false,
				'6': false,
				'7': false,
			},
			offspecFailover: true,
		},
```


### Rundown Management

Rundowns are stored as JSON files in the `rundowns` directory. The server watches this directory for changes:

- Adding a new `.json` file creates a new rundown
- Modifying a file updates the rundown
- Deleting a file removes the rundown

### Rundown JSON Format

Here's an example of a rundown JSON file:

```json
{
  "ID": "EXAMPLE_RO",
  "Slug": "Example Rundown",
  "DefaultChannel": "A",
  "Stories": [
    {
      "ID": "EXAMPLE_RO_STORY_1",
      "Slug": "Story 1",
      "Number": "1",
      "Items": [
        {
          "ID": "EXAMPLE_RO_STORY_1_ITEM_1",
          "Slug": "Item 1 in Story 1",
          "ObjectID": "OBJ_EXAMPLE_RO_STORY_1_ITEM_1",
          "MOSID": "DUMMY.MOS.SERVER",
          "ObjectSlug": "Item 1 in Story 1",
          "Duration": 1000,
          "TimeBase": 100
        }
      ]
    }
  ]
}
```

### Command-Line Interface

While the server is running, you can use the following commands:

- `outage [duration_ms]` - Simulate a server outage for the specified duration (default: 5000ms)
- `exit` - Shutdown the server and exit

Example: `outage 10000` will simulate a 10-second outage.

## Configuration

You can modify the configuration in the `SERVER_CONFIG` object in `src/index.ts`:

- `mosID`: The MOS ID of the server
- `acceptsConnections`: Whether to accept incoming connections
- `profiles`: The MOS profiles to support
- `debug`: Enable/disable debug logging
- `ports`: The ports to use for MOS communication

## Testing Failover

To test failover with your MOS client:

1. Start the server
2. Connect your MOS client to the server
3. Use the `outage` command to simulate an outage
4. Observe how your client handles the disconnection and reconnection
