# Instagram Unfollow Bot

> A stealthy Instagram unfollow bot with day/night mode, UI controls, and JSON user input support.  
> Includes an easy way to get your unfollow list of users not following you back.

---

## Features

- Automatically unfollow users from a JSON list  
- Day and night modes with adjustable delays and cooldowns  
- UI with progress, stop & restart controls  
- Paste your user list directly in the UI as JSON  
- Helpful instructions on how to generate your unfollow JSON file  

---

## Getting Your Unfollow List (Users Not Following You Back)

1. **Log in to Instagram on desktop.**

2. Open Developer Console:  
   - Windows/Linux: `Ctrl + Shift + J`  
   - Mac: `Cmd + Option + I`

3. Paste this code into the console and press Enter:

```js
function getCookie(b){let c=`; ${document.cookie}`,a=c.split(`; ${b}=`);if(2===a.length)return a.pop().split(";").shift()}
function sleep(a){return new Promise(b=>{setTimeout(b,a)})}
function afterUrlGenerator(a){return`https://www.instagram.com/graphql/query/?query_hash=3dec7e2c57367ef3da3d987d89f9dbc8&variables={"id":"${ds_user_id}","include_reel":"true","fetch_mutual":"false","first":"24","after":"${a}"}`}
let followedPeople,csrftoken=getCookie("csrftoken"),ds_user_id=getCookie("ds_user_id"),
initialURL=`https://www.instagram.com/graphql/query/?query_hash=3dec7e2c57367ef3da3d987d89f9dbc8&variables={"id":"${ds_user_id}","include_reel":"true","fetch_mutual":"false","first":"24"}`,
doNext=!0,filteredList=[],getUnfollowCounter=0,scrollCicle=0;
async function startScript(){
  for(;doNext;){
    let a;
    try{a=await fetch(initialURL).then(a=>a.json())}catch{continue}
    followedPeople||(followedPeople=a.data.user.edge_follow.count)
    doNext=a.data.user.edge_follow.page_info.has_next_page
    initialURL=afterUrlGenerator(a.data.user.edge_follow.page_info.end_cursor)
    getUnfollowCounter+=a.data.user.edge_follow.edges.length
    a.data.user.edge_follow.edges.forEach(a=>{
      a.node.follows_viewer||filteredList.push(a.node)
    })
    console.clear()
    console.log(`%c Progress ${getUnfollowCounter}/${followedPeople} (${parseInt(100*(getUnfollowCounter/followedPeople))}%)`,"background: #222; color: #bada55;font-size: 35px;")
    console.log("%c This users don't follow you (Still in progress)","background: #222; color: #FC4119;font-size: 13px;")
    filteredList.forEach(a=>{console.log(a.username)})
    await sleep(Math.floor(400*Math.random())+1000)
    scrollCicle++
    if(scrollCicle > 6){
      scrollCicle=0
      console.log("%c Sleeping 10 secs to prevent getting temp blocked","background: #222; color: ##FF0000;font-size: 35px;")
      await sleep(10000)
    }
  }
  const c=JSON.stringify(filteredList),
        d="usersNotFollowingBack.json",
        e="application/json",
        b=document.createElement("a"),
        f=new Blob([c],{type:e});
  b.href=URL.createObjectURL(f);
  b.download=d;
  b.click();
  console.log("%c All DONE!","background: #222; color: #bada55;font-size: 25px;");
}
startScript()
4. Wait for the script to finish. It will download a file called usersNotFollowingBack.json.

5. Upload that JSON file to https://43t6lx.csb.app/ and fliter/select the users you want to unfollow.

1. How to Use the Unfollow Bot
2. Open your browser console on Instagram (where you want to run the script).

3. Paste the bot script (the main script with the UI and controls).

4. You’ll see two popup boxes:

5. User JSON Input: Paste your filtered JSON list here (the one from the GUI tool).

6. Username List Textarea: Displays parsed usernames for quick review.

7. Click Start in the bot UI. The bot will:

8. Unfollow users one by one with delays based on day/night mode.

9. Show progress, let you stop or restart anytime.

**Important Notes**
The bot respects Instagram’s limits with cooldowns, but use responsibly.

Always backup your user list in localStorage — the bot does this automatically.

Adjust delays if you want to be extra cautious or faster.

If the bot stops unexpectedly, just restart it — it will resume from last progress.

You can customize the UI styles and bot behavior in the code.

How to Edit
To update your list of users, either paste new JSON in the input box or update localStorage key emergencyBackupList manually.

To change timing or cooldown, edit the delay values inside the startUnfollow function under day/night modes.

To customize UI, edit the createUI() and related UI update functions.
