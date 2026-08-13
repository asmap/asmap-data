# ASMap Data Repository

This repository holds recently created ASMap files encoded for use in Bitcoin Core. Any map included here has been created collaboratively between multiple participants (coordinated in an issue) and verified (in the follow-up pull request). The process is outlined in further detail on [Delving Bitcoin](https://delvingbitcoin.org/t/asmap-creation-process/548).

## Format

ASmap files are provided in binary form, suitable for use with Bitcoin Core's `-asmap` flag.

The files are organized by year. The `latest_asmap.dat` at the root of the project maps to the latest ASmap produced in these folders. An `_unfilled.dat` map means that only the networks sourced from the data are provided (see the [asmap-tool](https://github.com/bitcoin/bitcoin/blob/master/contrib/asmap/README.md) docs). The default is to use `--fill` when encoding the file to reduce the file size.

## Tools for analysis of maps

Users can further verify the integrity of the data with some of the [analysis commands](https://github.com/bitcoin/bitcoin/blob/master/contrib/asmap/README.md) available in `contrib/asmap/asmap-tool.py` (part of Bitcoin Core). There is also a ASMap health check log printed every 24 hours by Bitcoin Core nodes.

## Attestations

ASmap files can be attested to via PGP. In this repo:
- `attestations/<year>/<run epoch>/<signer>`: each ASmap from a collaborative run is run at a specific epoch (Unix timestamp), which serves to identify a given run.
  - `SHA256SUMS`: hashes of the `final_result.txt`, filled and unfilled encoded ASmap
  - `SHA256SUMS.asc`: detached PGP signature over the `SHA256SUMS` file
- `builder-keys/<signer>.gpg`: signer keys

The attestation file should contain three lines, the first for the final result, the second for the filled ASmap and the third for the unfilled ASmap, for example (assuming `EPOCH=1700000000000`):
```
cc199d5de04add6b5c2d95a72610c8a1a7b1f41fe01bd2b4c6db17795856aa31  final_result.txt
1146cbba8719cf3988d377df579667f68f97d2376d67755beb1e38194e196cfc  1700000000000_asmap_filled.dat
1c20ea2dee306af0a3ab4eaefaabe1e4c23a1c4256e60639e7ba48b2bbe56f24  1700000000000_asmap_unfilled.dat
```

### Script Usage

#### Attesting

To attest to an ASmap, you must have:
- the result file `final_result.txt` containing the ASmap in text format
- encoded the file via [`asmap-tool.py`](https://github.com/bitcoin/bitcoin/tree/master/contrib/asmap) as both filled and unfilled versions
- the Unix timestamp associated with the ASmap run
- a PGP key added to the `builder-keys` dir in this repo

Attesting to an ASmap output:
```bash
env SIGNER=<gpg-key-name>\
  ASMAP_TXT=<path/to/final_result.txt>\
  ENCODED_FILLED=<path/to/filled.dat>\
  ENCODED_UNFILLED=<path/to/unfilled.dat>\
  EPOCH=<unix_timestamp>\
  ./asmap-attest
```

This will add a `SHA256SUMS` file and a `SHA256SUMS.asc` file under the `<EPOCH>/<SIGNER>` folder.

#### Verifying

Verifying attestations in this repo:
```bash
./asmap-verify
```
or, for a specific epoch,
```bash
env EPOCH=177000000 \
  ./asmap-verify
```

This will print out verifications for the relevant attestations in the `attestations` dir.

## Note on reproducibility with kartograf

The map results added to the repository since December 2025 can be reproduced from the raw download files that are [published as release artifacts](https://github.com/bitcoin-core/asmap-data/releases).
However, reproduction requires using the right kartograf version that was used at the time to not receive a different result.
See the following table to pick the right version if you want to reproduce historical asmap files from the download files.

| Map timestamps | Kartograf version |
| --- | --- |
| <= 1786032000 | 0.4.13 (Gyoki) |
| > 1786032000 | 0.5.0 (Braun) |
