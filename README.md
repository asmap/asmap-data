# ASMap Data Repository

This repository holds recently created ASMap files encoded for use in Bitcoin Core. Any map included here has been created collaboratively between multiple participants (coordinated in an issue) and verified (in the follow-up pull request). The process is outlined in further detail on [Delving Bitcoin](https://delvingbitcoin.org/t/asmap-creation-process/548).

## Format

ASmap files are provided in binary form, suitable for use with Bitcoin Core's `-asmap` flag.

The files are organized by year. The `latest_asmap.dat` at the root of the project maps to the latest ASmap produced in these folders. An `_unfilled.dat` map means that only the networks sourced from the data are provided (see the [asmap-tool](https://github.com/bitcoin/bitcoin/blob/master/contrib/asmap/README.md) docs). The default is to use `--fill` when encoding the file to reduce the file size.

## Tools for analysis of maps

Users can further verify the integrity of the data with some of the [analysis commands](https://github.com/bitcoin/bitcoin/blob/master/contrib/asmap/README.md) available in `contrib/asmap/asmap-tool.py` (part of Bitcoin Core). There is also a ASMap health check log printed every 24 hours by Bitcoin Core nodes.
