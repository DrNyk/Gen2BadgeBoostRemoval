This is untested in Gen II, but I think this logic pans out. The game didn't break when I launched it for a playthrough, however, I don't have my tools set up to monitor the right memory addresses to be sure this worked as expected.

What patches get made in Gen II?

There is the same Stat Boosting as in Gen I, so we apply the same kind of hack as the Gen 1 files. At address 0x03EBB6 in Gold and at address 0x03ED7D in Crystal, we'll change some of the instructions for loading values and initiating the the divide by 2 commands to just load in 0s instead done by using "16 00 1E 00". The additional "00 00" are to remove one the second-half of a divide by 2 instruction; we overwrote only half of it. I guess it doesn't really matter and could be more elegant otherwise, but as tested in Gen I, I didn't want to break what wasn't broken.

However, Gen II introduces Type Boosting for all types except Dark type corresponding to the type specialty of the gym leader; and Blue's Earth Badge still boosting Ground. We need to remove that.

That is found at address 0xFBF67 in Gold/Silver and 0xFBE65 in Crystal (note the difference of 0x102 in addresses). We do the same kind of overwrite trick as it is calculating the +12.5% it wants to make, making it so it adds +0% instead. However, it seems there is a logic where if it would result in +0, it will rescue the boost and give it a +1 minimum. We want to get rid of that. So we just overwrite that +1 operation with 00 00 as well: At 0xFBF6F and 0xFBF70 in Gold/Silver, and at 0xFBE6D and 0xFBE6E in Crystal.
