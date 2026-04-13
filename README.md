# Secret Hitler — LAN Web App

A browser-based implementation of Secret Hitler for local multiplayer over WiFi. No installation, no server, no app store — just two HTML files.

Based on the original game by Mike Boxleiter, Tommy Maranges, and Mac Schubert, licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).

---

## Files

| File | Purpose |
|------|---------|
| `secret-hitler-host.html` | Host/iPad app — game master console |
| `secret-hitler-player.html` | Player app — runs on each phone |

---

## Setup & How to Play

### 1. Get everyone on the same WiFi
All devices must be on the same network, but the app also works over the internet since it uses WebRTC via PeerJS cloud signalling.

### 2. Host opens the host app (iPad recommended)
Open `secret-hitler-host.html` in any modern browser. A **5-character room code** will appear on screen once the connection initialises (takes a few seconds).

### 3. Players join on their phones
Each player opens `secret-hitler-player.html` on their phone browser, enters:
- Their **name**
- The **room code** shown on the host screen

Then taps **Join Game**.

### 4. Host starts the game
Once **5–10 players** have joined, the host taps **Start Game**. Roles are assigned secretly and each player's phone shows their role card privately.

### 5. Play follows the host screen
The iPad shows the shared game board. Players interact via their phones for private actions (voting, policy choices, etc.).

---

## Player Count & Role Distribution

| Players | Liberals | Fascists | Hitler |
|---------|----------|----------|--------|
| 5 | 3 | 1 | 1 |
| 6 | 4 | 1 | 1 |
| 7 | 4 | 2 | 1 |
| 8 | 5 | 2 | 1 |
| 9 | 5 | 3 | 1 |
| 10 | 6 | 3 | 1 |

In **5–6 player** games: Fascists and Hitler see each other during setup.  
In **7–10 player** games: Fascists see Hitler (thumb signal), but Hitler does not know who the fascists are.

---

## Win Conditions

**Liberals win if:**
- 5 Liberal policies are enacted, OR
- Hitler is assassinated

**Fascists win if:**
- 6 Fascist policies are enacted, OR
- Hitler is elected Chancellor after 3 or more Fascist policies have been enacted

---

## Game Flow

Each round has three phases:

### Election
1. The Presidential Candidacy passes clockwise
2. The President nominates a Chancellor (on their phone)
3. All players vote Ja!/Nein simultaneously (on their phones)
4. Majority Ja = government elected; tie or majority Nein = vote fails and Election Tracker advances

> If the Election Tracker reaches 3 failed votes in a row, the top policy is enacted automatically (no executive power granted), and term limits reset.

### Legislative Session
1. President draws 3 policy tiles, discards 1 (privately on their phone), passes 2 to Chancellor
2. Chancellor discards 1, enacts the remaining policy
3. Players may not communicate verbally during this phase — and may lie about what they saw

### Executive Action
When a Fascist policy is enacted, the President receives a one-use power (varies by player count and track position):

| Power | Effect |
|-------|--------|
| **Policy Peek** | President secretly views the top 3 tiles in the draw pile |
| **Investigate Loyalty** | President sees one player's Party Membership card (not their Secret Role). Each player may only be investigated once. |
| **Special Election** | President picks any player to be the next Presidential Candidate |
| **Execution** | President eliminates one player. If it's Hitler, Liberals win immediately. |

### Veto Power
Unlocked after 5 Fascist policies are enacted. The Chancellor may propose vetoing both policies — the President must consent. A successful veto counts as a failed government and advances the Election Tracker.

---

## Technical Details

**Networking:** Uses [PeerJS](https://peerjs.com/) (WebRTC) for peer-to-peer communication. The PeerJS cloud service (`0.peerjs.com`) handles the initial connection handshake only — all game data flows directly between devices. Works on LAN or over the internet.

**No server required.** Both files can be opened directly from the filesystem (`file://`) or served from any static file host.

**Browser support:** Any modern browser (Chrome, Firefox, Safari, Edge). Works on iOS Safari and Android Chrome.

**Offline play:** Requires internet access for the initial PeerJS handshake. Once connected, game data is peer-to-peer. If you need fully offline play, you can self-host a PeerJS server and update the `host`/`port`/`path` config in both HTML files.

### Self-hosting PeerJS (optional, for fully offline LAN)
```bash
npm install -g peer
peerjs --port 9000
```
Then in both HTML files, update the Peer constructor:
```js
new Peer('sh-host-' + code, {
  host: '192.168.x.x',  // your machine's LAN IP
  port: 9000,
  path: '/',
  secure: false,
  ...
})
```

---

## Tips for a Better Game

- **Put the iPad in the centre of the table** so everyone can see the board at a glance
- **Players should keep their phones face-down** except when taking an action — this prevents others from seeing their role card or policy choices
- **Use a phone stand** for the host iPad to free up table space
- The host app works fine on a laptop too — just less convenient for a shared table view

---

## Credits & License

Original game: **Secret Hitler** by Mike Boxleiter, Tommy Maranges, and Mac Schubert.  
Licensed under [Creative Commons Attribution-NonCommercial-ShareAlike 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).

This digital adaptation is a non-commercial fan implementation for personal use, shared under the same CC BY-NC-SA 4.0 licence terms.
