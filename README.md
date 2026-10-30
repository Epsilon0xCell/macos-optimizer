![Banner](banner.png)

# macos-optimizer

A menu-driven macOS optimisation script (`script.sh`), adapted from Sami Halawa's macOS optimiser with the author's own changes. Credit to the original project: https://github.com/samihalawa/macos-optimizer

## The Gatekeeper change, kept

The original script disabled Gatekeeper with `sudo spctl --master-disable`. This repo's version removes that line and instead enables Gatekeeper (menu option 23 runs `spctl --master-enable`). That change is kept as is.

## What the script does

On start it prints system information (hostname, macOS version, kernel, CPU, memory, storage), then loops a numbered menu:

1. Disable Spotlight indexing, 2. Disable sleep image, 3. Disable App Nap, 4. Disable automatic termination, 5. Run `fsck -fy`, 6. Enable TRIM (asks for confirmation, needs a restart), 7. Enable lid wake, 8. Optimise swap, 9. Disable sudden motion sensor, 10. Disable hibernation and sleep, 11. Flush DNS cache, 12. Rebuild Spotlight index, 13. Disable Dashboard, 14. Disable animations, 15. Disable local Time Machine snapshots, 16. Enable Secure Empty Trash, 17. Clear font caches, 18. Append a `.DS_Store` removal command to `~/.profile`, 19. Optimise network settings, 20. Optimise system performance, 21. Disable Bluetooth, 22. Enable TRIM, 23. Enable Gatekeeper, 24. Run only the safe set, 25. Run all optimisations except TRIM, 0. Quit.

Option 24, the safe set, runs Spotlight off, App Nap off, automatic termination off, `fsck -fy`, lid wake on, DNS flush, Spotlight rebuild, and Secure Empty Trash. After each action the script reports success or error and waits for Enter (or `q` to return to the menu).

## Fixes in this pass

- Option 18 and the matching step in "run all" appended `find. -name '.DS_Store' ...` to `~/.profile`. `find.` is not a command, so the appended line could never work. Both places now append `find . -name '.DS_Store' ...`.
- The menu label for option 24 listed item 6 (TRIM) among the safe optimisations, but the `run_safe` function does not run TRIM. The label now lists the set `run_safe` actually runs: 1, 3, 4, 5, 7, 11, 12, 16.
- The intro line that prints the upstream project URL used a `MAGENTA` colour variable that was never defined, so the URL printed without colour. `MAGENTA` is now defined alongside the other colours.

## Honest limits

The script's intro says no changes are made without explicit confirmation, but in fact choosing a menu item applies that change immediately (only TRIM asks first). That wording is left as written and recorded here rather than rebuilt with confirmations for all 25 options. The script is macOS only (it calls `sw_vers`, `sysctl` macOS keys, `pmset`, `mdutil`, and similar tools) and was verified here with `bash -n` only; it was not run live, because running it would change a real Mac's system settings.

Files: `script.sh`, `LICENSE`, `logo.png`, `banner.png`.
