# Smart Lighting

> Privacy-first smart lighting system. No cloud, no Alexa, no telemetry. Zigbee mesh network controlled entirely from the homelab via Home Assistant + Zigbee2MQTT.

---

## Why Not Philips Hue

- The Hue Bridge connects to Wi-Fi/Ethernet and phones home to Signify's cloud
- The Hue app requires cloud connectivity for features and updates
- You'd be trusting a corporation with your usage data
- The Hue button (ASIN: B0FCZRXH8Z) requires the proprietary Hue Bridge — non-starter

---

## The Protocol: Zigbee

- **IEEE 802.15.4** at 2.4 GHz — not Wi-Fi, not IP-based
- Devices have no IP addresses, don't connect to the router, can't phone home
- **AES-128 encryption** on all messages
- **Mesh networking** — mains-powered devices (bulbs) act as repeaters, extending range
- Battery devices (switches, sensors) are "end devices" — sleep and wake to send commands
- More bulbs = stronger, more reliable mesh

### Known Limitation

Traffic analysis of Zigbee packets (captured over the air) can identify device types and infer status — but requires physical proximity with a Zigbee sniffer. Niche threat for an apartment.

---

## Architecture

```
You tap a switch
    │
    ▼ (Zigbee 2.4 GHz radio)
Zigbee USB Coordinator (plugged into new living room server)
    │
    ▼ (USB serial)
Zigbee2MQTT (Docker container)
    │
    ▼ (MQTT publish)
Mosquitto Broker (Docker container)
    │
    ▼ (MQTT subscribe)
Home Assistant (Docker container)
    │
    ▼ (automation fires, command sent back down the chain)
Bulb turns on/off/dims
```

All local. All in milliseconds. Zero cloud. Zero internet dependency.

---

## Phase 1: Hardware

### Zigbee Coordinator (USB Dongle)

The dongle is a radio transceiver + microcontroller with an antenna. It acts as the **coordinator** — the single device that forms and manages the Zigbee mesh network. All devices join this coordinator's network.

Two main chips on the market:
- **Texas Instruments CC2652** — mature, well-documented
- **Silicon Labs EFR32MG21** — newer, also excellent

| Option | Chip | Antenna | Notes |
|--------|------|---------|-------|
| SONOFF ZBDongle-E | EFR32MG21 | External | Popular, cheap (~$13) |
| SMLight SLZB-06 | EFR32MG21 | External | Ethernet/PoE option, runs coordinator standalone |
| SONOFF ZBDongle-P | CC2652P | External | Older but rock solid |

**Recommendation**: SONOFF ZBDongle-E — cheapest, external antenna, great Zigbee2MQTT support.

### Switches (Zigbee, battery-powered)

| Option | Price | Notes |
|--------|-------|-------|
| IKEA TRADFRI Remote (E1810) | ~$15 | 5-button, well-supported |
| Aqara Mini Switch (MCCGQ11LM) | ~$12 | Single button, tiny |
| SONOFF SNZB-01P | ~$10 | Single button, cheapest |
| IKEA SOMRIG Shortcut Button | ~$10 | 2-button, newer |

### Bulbs: 2200K Edison Style (Dumb) + Zigbee Smart Plugs

Standard Zigbee bulbs don't come in 2200K amber Edison style. The solution: use **dumb Edison bulbs** paired with **Zigbee smart plugs**. The plug gives on/off control and acts as a mesh repeater.

#### Bulb Choice: Brightever ST58 Edison (winner)

| | DiCUNO ST64 | Brightever ST58 |
|---|---|---|
| CRI | 80 | **90+** |
| Lumens | 600lm | **850lm** |
| Dimmable | No | **Yes** |
| Price | ~$5/bulb (2-pack) | **~$3.67/bulb (6-pack)** |
| Shape | ST64 (larger globe) | ST58 (classic Edison) |

**Go with Brightever.** Higher CRI, brighter, dimmable, cheaper.

#### What is CRI?

CRI (Color Rendering Index) measures how accurately a light reveals true colors vs sunlight (CRI 100). At 2200K warm amber:
- **CRI 80** — reds and skin tones look washed out, wood grain appears flat
- **CRI 90+** — warm tones render faithfully, wood/leather/skin look rich and natural
- The difference is subtle but you *feel* it in a living space — everything looks more alive

#### Zigbee Smart Plugs (control the dumb bulbs + mesh repeaters)

| Option | Price | Notes |
|--------|-------|-------|
| SONOFF ZBMINI-L2 | ~$10 | Inline switch, compact |
| IKEA TRETAKT | ~$10 | Simple plug, well-supported |
| SONOFF S31 Lite ZB | ~$12 | Outlet plug, power monitoring |

Plug a lamp into the Zigbee plug → plug controls on/off → plug also repeats mesh signal. You lose smart dimming (on/off only), but for ambient Edison bulbs at a fixed warm glow, on/off is all you need.

#### Alternative: Zigbee Bulbs (for rooms where you want dimming/color control)

| Option | Price | Notes |
|--------|-------|-------|
| IKEA TRADFRI LED (E27/E26) | ~$8 | Cheap, reliable, dimmable |
| IKEA TRADFRI Color (E26) | ~$18 | RGB + white spectrum |
| Aqara LED Bulb T1 | ~$15 | Tunable white |

### Bill of Materials

| # | Item | Qty | Budget | Status | Link |
|---|------|-----|--------|--------|------|
| 1 | SONOFF ZBDongle-E (EFR32MG21 Coordinator) | 1 | $23.51 | Purchased | [Amazon](https://www.amazon.com/ZBDongle-Universal-Coordinator-Zigbee2MQTT-Automation/dp/B0FNLP58JS) |
| 2 | Brightever ST58 2200K Edison Bulbs (6-pack, 90+ CRI, dimmable) | 1 | ~$22 | To buy | [Amazon](https://www.amazon.com/Brightever-Equivalent-Dimmable-Decorative-Lightbulbs/dp/B0F3JHP5FK) |
| 3 | SONOFF S31 Lite ZB Zigbee Smart Plug (on/off + mesh repeater) | 3 | ~$20/ea | To buy | [Amazon](https://www.amazon.com/SONOFF-Zigbee-SmartThings-Amazon-Needed/dp/B082PSKRSP) |
| 4 | SONOFF SNZB-01P Zigbee Wireless Switch (2-pack) | 1 | ~$16 | To buy | [Amazon](https://www.amazon.com/SONOFF-SNZB-01P/dp/B0FQTYN5ZV) |
| 5 | Lamp — Kallax shelf (E26, compact, Edison-style) | 1 | ~$15-25 | TBD | Searching |
| 6 | Lamp — Bedside left (E26, small table/nightstand lamp) | 1 | ~$15-25 | TBD | Searching |
| 7 | Lamp — Bedside right (E26, small table/nightstand lamp) | 1 | ~$15-25 | TBD | Searching |

#### Lamp Ideas

**For Kallax** — needs to be compact enough to sit on or inside the 8x4 shelf. Options:
- [OuXean Small Wood Table Lamp](https://www.amazon.com/OuXean-Edison-Dimmable-Industrial-Bedroom/dp/B09B239HH5) — tiny wood base, exposed Edison, ~$20
- [IKEA FADO Table Lamp](https://www.amazon.com/IKEA-FADO-Table-Lamp-White/dp/9178896584) — orb shape, but hides the Edison aesthetic
- A simple cage/pendant lamp sitting on top of the Kallax — best for showing off the filament

**For Bedsides** — matching pair for the long drawer / nightstands:
- [Small Metal Lamp Base Set of 2](https://www.amazon.com/Industrail-Bedroom-Lamp-Base-Nightstand/dp/B09NDLFRMZ) — minimal black metal, E26, industrial, ~$20 for 2
- [Walsport Concrete Edison Lamp](https://www.amazon.com/Walsport-Concrete-Nightstand-Industrial-Christmas/dp/B081JSDSH4) — concrete base, minimal industrial, ~$15
- [YUANKANG Edison Desk Lamp](https://www.amazon.com/YUANKANG-Vintage-Industrial-Bedside-Lighting/dp/B08HXKF9KS) — vintage black base, exposed bulb, ~$12

#### Budget Summary

| Category | Est. Cost |
|----------|-----------|
| Coordinator (purchased) | $23.51 |
| Edison bulbs (6-pack) | ~$22 |
| Zigbee smart plugs (3x) | ~$60 |
| Zigbee switches (2-pack) | ~$16 |
| Lamps (3x) | ~$45-75 |
| **Total** | **~$165-195** |

---

## Floor Plan & Lighting Placement

![[Floor Plan.png]]

### Apartment Layout

| Room | Size | Position |
|------|------|----------|
| Bedroom (mine) | 91.60 sq ft | Top — has long drawer along wall |
| Bathroom | 30.47 sq ft | Middle left |
| Kallax nook | 33.46 sq ft | Middle — IKEA Kallax 8x4 shelf unit |
| Living Room | 565.04 sq ft | Center — main open space |
| Office | 90.79 sq ft | Bottom — existing TrueNAS server lives here |

### Device Placement Plan

```
BEDROOM (top)
  └─ Edison bulb on long drawer lamp ← Zigbee plug
     (end device — relays through Kallax bulb)
        │
        │ ~15 ft through walls
        ▼
KALLAX NOOK (middle)
  └─ Edison bulb on Kallax shelf lamp ← Zigbee plug
     KEY REPEATER — bridges bedroom ↔ living room
        │
        │ ~10 ft open air
        ▼
LIVING ROOM (center)
  └─ 1-2 Edison bulbs on floor/table lamps ← Zigbee plug(s)
     (repeaters + main ambient lighting)
        │
        ▼
NEW SERVER (at TV, right side)
  └─ Coordinator dongle plugged in here
        │
        │ ~20 ft
        ▼
OFFICE (bottom) — optional coverage, not priority
```

### Why This Layout Works

- **Kallax bulb is the mesh backbone** — sits perfectly between bedroom and living room, relays signals through the walls so nothing depends on a direct shot to the coordinator
- **Living room bulbs** provide primary ambient lighting and strengthen the mesh core
- **Bedroom bulb** on the long drawer is the farthest node but only needs to reach the Kallax repeater (~15 ft), not the coordinator directly
- **Server at TV** — coordinator has line-of-sight to living room bulbs and the Kallax area
- **No USB extension cable needed** — mesh handles the distance, apartment is compact

### Interior Design Notes

- Edison bulbs on the Kallax look great in a table lamp, cage lamp, or hanging pendant sitting on top of the shelf
- Living room: floor lamps or table lamps with exposed Edison bulbs for warm ambient glow
- Bedroom: desk/drawer lamp — one bulb is enough for the 92 sq ft space
- Consistent 2200K amber throughout creates a cohesive warm aesthetic across the apartment

---

## Phase 2: Software Stack (Docker on TrueNAS)

### Services

| Service | Image | Purpose |
|---------|-------|---------|
| mosquitto | `eclipse-mosquitto:2` | MQTT broker — message bus between Zigbee2MQTT and HA |
| zigbee2mqtt | `koenkk/zigbee2mqtt:latest` | Translates Zigbee radio ↔ MQTT messages |
| homeassistant | `ghcr.io/home-assistant/home-assistant:stable` | Web UI, automations, device management |

### 2a. Add to homelab-infra

- [ ] Add the three services to `homelab-infra/docker-compose.yml`
- [ ] Create config directories:
  ```
  homelab-infra/services/smart-lighting/
  ├── mosquitto/
  │   └── mosquitto.conf
  ├── zigbee2mqtt/
  │   └── configuration.yaml
  └── homeassistant/
      └── (auto-generated on first run)
  ```

### 2b. Mosquitto Config

- [ ] Write `mosquitto.conf`:
  - Listener on port 1883 (local only)
  - `allow_anonymous true` (internal network only, no external exposure)
  - Persistence enabled

### 2c. Zigbee2MQTT Config

- [ ] Write `configuration.yaml`:
  - MQTT server: `mqtt://mosquitto:1883`
  - Serial port: `/dev/ttyUSB0` (or wherever the dongle enumerates)
  - Frontend enabled on port 8080 (local access only)
  - `permit_join: false` (enable temporarily when pairing new devices)
  - Home Assistant MQTT discovery enabled

### 2d. Home Assistant Config

- [ ] First run generates default config
  - MQTT integration auto-discovers Zigbee2MQTT devices
  - Create automations via the UI or `automations.yaml`

---

## Phase 3: Deploy

- [ ] Plug the Zigbee USB dongle into the new living room server (at TV)
- [ ] Verify dongle appears: `ls /dev/ttyUSB*` or `ls /dev/ttyACM*`
- [ ] Pass the USB device through to the Zigbee2MQTT container (`devices:` in compose)
- [ ] `docker compose pull && docker compose up -d`
- [ ] Verify all three containers are healthy: `docker compose ps`
- [ ] Access Home Assistant at `http://<server-ip>:8123`
- [ ] Access Zigbee2MQTT frontend at `http://<server-ip>:8080`

---

## Phase 4: Pair Devices

- [ ] In Zigbee2MQTT frontend, enable `permit_join`
- [ ] Put each bulb into pairing mode (usually: toggle power 5-6 times rapidly)
- [ ] Put each switch into pairing mode (varies by device — check Zigbee2MQTT docs)
- [ ] Verify devices appear in Zigbee2MQTT and auto-discover in Home Assistant
- [ ] Disable `permit_join` when done

---

## Phase 5: Automations

- [ ] In Home Assistant, create automations:
  - Switch button press → toggle specific bulb(s)
  - Long press → dim/brighten
  - Double press → scene change (e.g., movie mode, reading mode)
- [ ] Test all switch-to-bulb bindings

---

## Phase 6: Remote Access (via Tailscale)

- [ ] Home Assistant is already accessible through Tailscale mesh (TrueNAS is on Tailscale)
- [ ] Access HA at `http://<tailscale-ip>:8123` from anywhere
- [ ] No Cloudflare tunnel needed — Tailscale is already the private overlay network
- [ ] Zero internet exposure. Zero cloud dependency.

---

## Phase 7: Lockdown

- [ ] Firewall rules: Zigbee2MQTT and Mosquitto containers should have NO internet access
- [ ] Home Assistant: disable cloud integration, analytics, and any call-home features
- [ ] Zigbee2MQTT: verify `availability` and `ota.disable_automatic_update_check: true`
- [ ] No ports exposed externally — everything accessed via Tailscale only

---

## Phase 2 Ideas (Future Upgrades)

### Upgrade: Zigbee Dimmer Modules (replace smart plugs)

The Brightever Edison bulbs are dimmable — they just need something that modulates power. Zigbee smart plugs are on/off only, but **Zigbee dimmer modules** wire inline and provide full dimming control.

**How it works**: cut the lamp cord, wire the dimmer module inline (live in, live out, neutral), tuck it into the lamp base or a small enclosure. 10-minute job per lamp.

```
Wall outlet → lamp cord → Zigbee dimmer module → Brightever Edison bulb
                              │
                              └─ Zigbee mesh (repeater + dimmable)
```

| Option | Price | Notes |
|--------|-------|-------|
| MOES Zigbee 3.0 Dimmer Module | ~$12-15 | Well-supported in Zigbee2MQTT, trailing-edge (LED-friendly) |
| Tuya Zigbee Dimmer Module | ~$10-15 | Various brands on AliExpress, same concept |

**Strategy**: start with Zigbee smart plugs (on/off) to get the system running. Live with it. If dimming is desired, swap plugs for inline dimmer modules — 1-for-1 replacement, no other changes to the stack.

### Upgrade: DIY Corner/Floor Lamp

From the r/homeassistant community — buy a cheap LED corner lamp off Amazon, rip out the electronics, replace with:
- **Zigbee LED controller** (like the LM052) for Zigbee mesh integration
- **WLED on ESP32** for WiFi-based addressable LED control (more effects, not Zigbee)
- **ESP32-C6** has Zigbee/Thread built in but ESPHome doesn't officially support Zigbee yet

The Zigbee LED controller route keeps everything on the Zigbee mesh. The WLED route gives more effects but adds a WiFi device. Either way, it's a fun project and the community has tons of builds.

### Upgrade: Zigbee Smart Bulbs (for specific rooms)

If dimming matters more than Edison aesthetics in certain spots (e.g., bedroom for sleep), swap in a Zigbee smart bulb:
- IKEA TRADFRI dimmable — goes down to ~2200K at lowest dim
- These still act as mesh repeaters

---

## Network Map

| Device | Protocol | Network | Location |
|--------|----------|---------|----------|
| Zigbee smart plugs | Zigbee 2.4 GHz | Zigbee mesh (NOT Wi-Fi) | Bedroom, Kallax, Living Room |
| Zigbee switches | Zigbee 2.4 GHz | Zigbee mesh (NOT Wi-Fi) | Bedroom, Living Room |
| Edison bulbs | Dumb (no radio) | Powered via Zigbee plugs | All rooms |
| USB Coordinator | USB serial | Wired to server | Living room (at TV) |
| Zigbee2MQTT | TCP (MQTT) | Docker internal network | Living room server |
| Mosquitto | TCP :1883 | Docker internal network | Living room server |
| Home Assistant | TCP :8123 | Docker + Tailscale | Living room server |

---

## Key Principles

- **No cloud accounts needed** — everything runs locally
- **No Wi-Fi for devices** — Zigbee operates on its own radio network
- **No vendor lock-in** — Zigbee 3.0 devices from any brand work with Zigbee2MQTT
- **No internet dependency** — lights work even if ISP goes down
- **You control your lights** — full ownership of the stack
