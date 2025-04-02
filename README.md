# DataMastiff

Mastiffs are strong and reliable, like this sync extension for your browser.

## Rationale

Why am I building this?

For the past couple of months I've been looking for an alternative to the nonsense that Google decided to implement with their MV3 changes to extensions for the chromium browsers. The lead me to FireFox. Now, near the end of Feburary 2025, Mozilla didn't handle their new change with an introduction of Terms of Use to FireFox well in the slightest. Will anything actually change? I'm not sure, and I really don't think that people care that much about it in the long run. But because Mozilla have introduced this change, I do not personally want to use FireFox anymore. So, I'm using a fork of FireFox, so that I'm not impacted by the MV3 changes. But now. With this, I also don't want to support Mozilla in any way, at all, and in order for me to get "browser" sync functionality with FireFox or a fork thereof, I would need to create a Mozilla account. This isn't something that I want to do. So, that leaves me with these thoughts:

I'm looking for some third party open source alternatives to the firefox browser sync setting. I think that the standard firefox sync feature only sync's certain information (anyway, it doesn't seem to sync the ENTIRE browser's state), and some alternatives that I've looked at seem to focus on sync'ing bookmarks only (with some of them sync'ing tabs). But I'd love a firefox alternative that will sync basically all the firefox settings - i.e. the ones that are in the profile (folder that are linked to your instance on your device), so that if I log into a new device, I can sync everything, the browser the way it is - that being the addons, bookmarks, and other custom settings that I've made in the about:config for example, as well as the actual settings within an extension for the browser.

This is quite a specific requirement that I'm not sure if a current solution even currently exists out there, because it's quite an "all encompassing sync", that I'm looking for. Also, any open source code, while can be reviewed, is usually very very large in scope, and to make sure that there ism't any fishy business happening in the code is quite an undertaking of its own. So. I am more inclined to develop this myself. I will focus on firefox (and it's forks) for now, because I'm currently using Floorp as my main browser, and I want to basically take everything from the profile (that is stored on the disk), encrypt it possibly and store it on a "sync server" so that it can be pulled by the extension on another pc. But, I need this to work specifically on the browser and not be platform specific, because my work pc is mac and my personal pc switches between windows and linux. So, just putting the files there might be tricky.
A third party extension that is open source would be nice to look at, but I don't know if there are any third party tools/extensions that include the entire scope that I'm looking for.

I think what we could do is start with the basic sync. So, this would basically just "back up" the current browser's information to a "server" and then pull that data. Since I'm gonna be using it myself and be the owner of everything, it doesn't need to be encrypted as the MVP. I just want to start a new project that I can open source, that will add these type of features along the way. But the bare bones of the MVP is that I want all of the browser settings (those configurable in about:preferences, across all sections), the bookmarks and history (and ideally) the current extensions and their settings.

Maybe this task isn't too "bad", because if I remember correctly, the firefox data is mostly stored in a sqlite database in the profile directory. With some things being stored in text files, json files, etc.

This will be the first fully fledged extension that I create, so expect some "weirdness", I guess, as I learn more about how to do it.

## Basic overview

### Understanding Firefox Profile Storage

Firefox stores user data in a profile folder, typically located at:

- Windows: C:\Users\<USER>\AppData\Roaming\Mozilla\Firefox\Profiles\
- Linux: ~/.mozilla/firefox/
- Max: /Users/<USER>/Library/Application Support/Firefox/Profiles

Within this folder, relevant files include:

- Bookmarks & History → places.sqlite (SQLite database)
- Extensions → extensions.json (List of installed extensions)
- Extension Settings → browser-extension-data/ (Stores per-extension settings)
- About:Config Settings → prefs.js (JavaScript-formatted preference file)
- Session & Tabs → sessionstore.jsonlz4 (Compressed JSON file)

I will need to make sure that these are the basic files that are "sync"d across. Ideally, I want to just take everything, but I need to see what's possible to take over.

### Feasibility of a Firefox Extension

Firefox extensions follow the WebExtensions API, which cannot directly access arbitrary files (for security reasons). However, you can:

- Use the browser.storage API to store small amounts of settings in a local database.
- Use the bookmarks, history, and tabs APIs to extract and sync that data.
- Use native messaging to communicate with an external script running on the system (this could handle profile file access).
- Package an external companion app to read profile data and sync it.
