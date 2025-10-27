# Quick-MOS

An application for quick simulation of a MOS server/NCS.

## Usage

```bash
git clone https://github.com/Sofie-Automation/sofie-mos-connection.git
cd sofie-mos-connection

yarn install # Install dependencies
yarn build # Build all mos-connection packages

cd /packages/quick-mos

yarn start # Start the Quick-MOS application
```

- The application will monitor the contents in the folder `/input` and send mos commands.
- Files and folders that begin with "\_" (underscore) will be ignored

- Note: If you run Quick-MOS and a [MOS-Gateway](https://github.com/Sofie-Automation/sofie-core/tree/main/packages/mos-gateway), they must be run on different machines (or docker containers) as they both try to bind to the same ports. This is a limitation in the current implementation of mos-connection.
