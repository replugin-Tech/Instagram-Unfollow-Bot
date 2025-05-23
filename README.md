💀 Instagram Unfollow Bot
A stealthy Instagram unfollow bot with day/night mode, UI controls, and JSON user input support.

Includes an easy way to get your unfollow list of users not following you back.

✨ Features
Automatically unfollow users from a JSON list

Day and night modes with adjustable delays and cooldowns

Sleek UI with progress, stop, and restart buttons

Paste your user list directly in the UI as JSON

Auto-resume from last saved progress

LocalStorage backup for user list

Built-in safety pauses to avoid detection

Optionally likes random posts or views stories (stealth actions)

🔍 Getting Your Unfollow List (Users Not Following You Back)
This script scans all the people you follow and finds the ones who don’t follow you back. It downloads a JSON file that you’ll feed into the bot.

🛠️ Instructions:
Login to Instagram on desktop and go to your profile page.

Open the Developer Console:

Windows/Linux: Ctrl + Shift + J

Mac: Cmd + Option + I

Paste the script below into the console and press Enter:

js
Copy
Edit
function getCookie(b) {
  let c = `; ${document.cookie}`,
      a = c.split(`; ${b}=`);
  if (a.length === 2) return a.pop().split(";").shift();
}

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

function afterUrlGenerator(after) {
  return `https://www.instagram.com/graphql/query/?query_hash=3dec7e2c57367ef3da3d987d89f9dbc8&variables={"id":"${ds_user_id}","include_reel":"true","fetch_mutual":"false","first":"24","after":"${after}"}`;
}

let followedPeople,
    csrftoken = getCookie("csrftoken"),
    ds_user_id = getCookie("ds_user_id"),
    initialURL = `https://www.instagram.com/graphql/query/?query_hash=3dec7e2c57367ef3da3d987d89f9dbc8&variables={"id":"${ds_user_id}","include_reel":"true","fetch_mutual":"false","first":"24"}`,
    doNext = true,
    filteredList = [],
    getUnfollowCounter = 0,
    scrollCicle = 0;

async function startScript() {
  while (doNext) {
    let res;
    try {
      res = await fetch(initialURL).then(r => r.json());
    } catch {
      continue;
    }

    if (!followedPeople) followedPeople = res.data.user.edge_follow.count;

    doNext = res.data.user.edge_follow.page_info.has_next_page;
    initialURL = afterUrlGenerator(res.data.user.edge_follow.page_info.end_cursor);
    getUnfollowCounter += res.data.user.edge_follow.edges.length;

    res.data.user.edge_follow.edges.forEach(edge => {
      if (!edge.node.follows_viewer) filteredList.push(edge.node);
    });

    console.clear();
    console.log(`%c Progress ${getUnfollowCounter}/${followedPeople} (${parseInt(100 * (getUnfollowCounter / followedPeople))}%)`, "background: #222; color: #bada55; font-size: 35px;");
    console.log("%c These users don't follow you back (Still fetching...)", "background: #222; color: #FC4119; font-size: 13px;");
    filteredList.forEach(user => console.log(user.username));

    await sleep(Math.floor(400 * Math.random()) + 1000);

    scrollCicle++;
    if (scrollCicle > 6) {
      scrollCicle = 0;
      console.log("%c Sleeping 10 secs to prevent getting temp blocked", "background: #222; color: #FF0000; font-size: 35px;");
      await sleep(10000);
    }
  }

  const data = JSON.stringify(filteredList),
        filename = "usersNotFollowingBack.json",
        mimeType = "application/json",
        link = document.createElement("a"),
        blob = new Blob([data], { type: mimeType });

  link.href = URL.createObjectURL(blob);
  link.download = filename;
  link.click();

  console.log("%c All DONE!", "background: #222; color: #bada55; font-size: 25px;");
}

startScript();
Wait for the script to finish — it’ll automatically download a file called usersNotFollowingBack.json.

Go to https://43t6lx.csb.app/ and upload the JSON file.

Use the GUI there to filter/select which users you want to unfollow.

🤖 How to Use the Unfollow Bot
Open Instagram on desktop and go to your profile.

Open the browser Developer Console.

Paste the bot script (the one with the UI and unfollow logic).

Two input boxes will appear:

JSON Input Box — paste your filtered user list from the previous step.

Username Preview Box — shows a preview of usernames.

Click Start in the UI.

The bot will:

Unfollow users one by one

Obey day/night delay rules

Update progress in real-time

Let you stop or restart anytime

⚠️ Important Notes
The bot respects Instagram limits with cooldowns, but still — don’t go wild.

Your filtered list is backed up to localStorage under the key emergencyBackupList.

If the bot is interrupted, it will auto-resume from the last point.

You can customize UI styles and delay values in the source code.

🛠️ How to Customize
To update user list: Paste a new JSON list into the UI or manually update localStorage under the key emergencyBackupList.

To change timing/cooldowns: Edit the startUnfollow() function.

To tweak UI: Edit the createUI() and its helper functions.

