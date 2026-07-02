# EEGLAB conversion script

`edf_to_eeglab.m` converts the released BIDS EDF files into EEGLAB `.set` files
on demand, so the dataset does not ship a duplicate copy of the data.

## Why a script instead of bundled .set files
The repository root EDFs are the canonical data. EEGLAB reads them directly
(via the BIOSIG plugin), so a parallel `.set` copy would only duplicate ~85 MB
with no new information. This script regenerates `.set` files locally in a
minute, with events and the standard 10-20 channel locations already attached.

## Requirements
- MATLAB-based EEGLAB (the standalone/compiled EEGLAB cannot run scripts).
- The BIOSIG plugin (EEGLAB > File > Manage extensions > BIOSIG).

## Usage
1. In MATLAB: `>> eeglab`
2. Set `BIDS_ROOT` at the top of `edf_to_eeglab.m` to the dataset root
   (the folder containing `sub-01`, `sub-02`, …), or `cd` there first.
3. `>> edf_to_eeglab`

Output: `derivatives/eeglab/sub-NN_task-foodcat_eeg.set`, one per subject.
No filtering, resampling, or re-referencing is applied — the signal matches
the released EDF exactly.

## Notes
- Users of MNE-Python or FieldTrip can work directly from the EDF + events.tsv
  and do not need this script.
- Some general-purpose EDF readers misparse the EMOTIV header and return flat
  signals; BIOSIG parses it correctly.
