# <img src="/favicons/favicon-96x96.png?raw=true" height="32px" alt=""/> Apple Podcast Transcript Viewer

This is a simple UI tool for viewing full transcripts in a way that you can actually copy them. No software needed and it should work with any Mac with the Podcasts app just by visiting the website, https://alexbeals.com/projects/podcasts/. All you have to do is follow the instructions and drag-and-drop your podcasts folder.

This all works locally without uploading anything, which you can confirm by disabling the Internet after the website is loaded. 

<img width="1368" alt="Screenshot 2025-01-30 at 11 31 52 PM" src="https://github.com/user-attachments/assets/683f252d-4255-47d5-9e15-2c747ffefb68" />

Once you drag and drop your files you can browse all of the episodes with transcripts.

<img width="1368" alt="Screenshot 2025-01-30 at 11 31 47 PM" src="https://github.com/user-attachments/assets/a108a39a-2fbf-4971-a1ee-336d2ec45e9e" />

Clicking on any of them will pull up the full transcript, which you can copy and paste to whatever tool you want to handle the file in.

<img width="1144" alt="Screenshot 2025-01-31 at 12 08 35 AM" src="https://github.com/user-attachments/assets/0985f961-b661-4679-b6db-026539fa7062" />

## Where Does This Come From?

The data is locally stored in `~/Library/Group Containers/243LU875E5.groups.com.apple.podcasts/Library/Cache/Assets/TTML`. The tool also pulls in the `.sqlite` folder to display additional information about the podcast to make it easier to find the one you're looking for. Shoutout to @mattdanielmurphy and his [repo here](https://github.com/mattdanielmurphy/apple-podcast-transcript-extractor) which I found when originally trying to do this for a podcast.

## Local Debugging
Loading files doesn't work just by opening from `file://` in your browser, so instead use Python to set up a local server at http://localhost:8000/.

```
python3 -m http.server
```

The "tests" folder also contains .zip files that should continue to work.

## How did you get WAL working with sql.js??

Great question. This [issue was the key](https://github.com/sql-js/sql.js/issues/372). But the compiling steps were a nightmare, so I just manually modified the `sql-wasm.js` file. Will need to do this again with a version boost. Specifically you can look for the `dbfile_` bit in code, find the `if(null!=g)` code and copy it with a different variable (and the '-wal' suffix in the filename definition).

Original:
```
function e(g){this.filename="dbfile_"+(4294967295*Math.random()>>>0);if(null!=g){var l=this.filename,n="/",t=l;n&&(n="string"==typeof n?n:ja(n),t=l?x(n+"/"+l):
n);l=ka(!0,!0);t=la(t,(void 0!==l?l:438)&4095|32768,0);if(g){if("string"==typeof g){n=Array(g.length);for(var w=0,A=g.length;w<A;++w)n[w]=g.charCodeAt(w);g=n}ma(t,l|146);n=na(t,577);oa(n,g,0,g.length,0);pa(n);ma(t,l)}}
```

Replacement:
```
function e(g,zzz){this.filename="dbfile_"+(4294967295*Math.random()>>>0);if(null!=g){var l=this.filename,n="/",t=l;n&&(n="string"==typeof n?n:ja(n),t=l?x(n+"/"+l):
n);l=ka(!0,!0);t=la(t,(void 0!==l?l:438)&4095|32768,0);if(g){if("string"==typeof g){n=Array(g.length);for(var w=0,A=g.length;w<A;++w)n[w]=g.charCodeAt(w);g=n}ma(t,l|146);n=na(t,577);oa(n,g,0,g.length,0);pa(n);ma(t,l)}}if(null!=zzz){var l=this.filename+"-wal",n="/",t=l;n&&(n="string"==typeof n?n:ja(n),t=l?x(n+"/"+l):
n);l=ka(!0,!0);t=la(t,(void 0!==l?l:438)&4095|32768,0);if(zzz){if("string"==typeof zzz){n=Array(zzz.length);for(var w=0,A=zzz.length;w<A;++w)n[w]=zzz.charCodeAt(w);zzz=n}ma(t,l|146);n=na(t,577);oa(n,zzz,0,zzz.length,0);pa(n);ma(t,l)}}
```