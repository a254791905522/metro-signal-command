# Metro Signal Command - Gameplay Fact Card

## App Identity
- **App Name**: Metro Signal Command
- **Bundle ID**: com.rosewood.driftsubway.game
- **Version**: 1.0
- **Platform**: iOS (iPhone only)
- **Minimum iOS**: 15.0
- **Framework**: SpriteKit + Objective-C
- **Orientation**: Portrait only
- **Age Rating**: 4+
- **Network**: Fully offline, no ads, no IAP, no analytics, no tracking

## Core Gameplay Loop
1. Level starts; the first train is auto-selected (GameScene.m: `if (self.trainStates.count > 0) [self selectTrainAtIndex:0];`).
2. A 60-second countdown begins. Trains depart after `kDefaultDwellTime + scheduleOffset` seconds.
3. Player taps trains to select, taps +/brake to adjust speed (+0.05 per tap, range 0.01 to 2x base).
4. Trains enter speed-limit zones; speeding triggers auto-brake and -8 points. Safe passage awards +2 points/second (combo).
5. Trains too close (under `safeFollowingDistance`) trigger rear auto-brake and -10 points.
6. Faults (signal/door/crowd) appear at L10/L20/L30/L40/L50; player taps Resolve Fault; faster resolution = higher score.
7. Timer ends; level scores on punctuality (40%), fault response (30%), satisfaction (30%), plus bonus; stars awarded (3 at 80, 2 at 60, 1 at 40).
8. Progress saved to NSUserDefaults `ds_unlocked_level` and `ds_stars_{n}`.

## Trains
- Visual: 28x12 rectangle body + 8x12 triangle nose, direction-rotated.
- States: white (normal), yellow (speed warning), red (speeding), orange (too close).
- Selection: yellow highlight box.
- Count per level: 1 (L1-L10), 2 (L11-L30), 3 (L31-L40), 4 (L41-L50).
- Dispatch offset: staggered (0/4/5/6/8/10/12 seconds) to avoid initial collision.

## Stations
- Visual: 8px outer circle + 5px inner circle, black border.
- Station name: 9pt gray text below station.
- Layout: stations distributed at i/(N-1) along the line.
- Total across 50 levels: 1372 real stations.

## Level Progression
- 50 levels, 5 geographic chapters:
  - Europe (L1-L15): 1-2 trains, cities incl. London, Paris, Berlin, Moscow, Rome, Madrid.
  - Asia (L16-L30): 2 trains, cities incl. Istanbul, Tehran, Dubai, Karachi, Delhi, Bangkok, Seoul, Tokyo, Beijing.
  - Africa (L31-L36): 2-3 trains, cities incl. Cairo, Lagos, Nairobi, Johannesburg.
  - North America (L37-L44): 3 trains, cities incl. New York, Toronto, Mexico City, Los Angeles, Chicago.
  - South America + Oceania (L45-L50): 4 trains, cities incl. Sao Paulo, Buenos Aires, Santiago, Sydney.
- Each level is a real metro line with real station names (e.g., London Metropolitan 34 stations, Seoul Subway Line 1 93 stations).
- Persistent unlock: clearing a level sets `ds_unlocked_level` and `ds_stars_{n}`.

## Scoring
- Punctuality (40%): `(1 - avgDeviation) * 40`.
- Fault response (30%): `(1 - resolveTime/10s) * 30`, 0 if unresolved.
- Satisfaction (30%): 30 default, 15 during a fault.
- Combo bonus: `runningScore / 2`, capped at 30.
- Total: `min(100, punctuality + fault + satisfaction + bonus)`.
- Stars: 3 at >=80, 2 at >=60, 1 at >=40.

## Faults
- signal: all trains speed cap to 0.04 (forced slowdown).
- door: station dwell time +3 seconds.
- crowd: station dwell time +1.5 seconds.
- Triggered at L10, L20, L30, L40, L50 only.

## Speed-Limit Zones
- Yellow (warning): speed 0.03-0.05.
- Red (enforced): speed 0.02-0.04.
- Speeding: auto-brake + -8 points.
- Safe passage: +2 points/second, combo counter.

## Monetization
- None. No ads, no in-app purchases, no subscriptions.

## Data Collection
- None. All progress (highest unlocked level, stars per level, music/sound toggles) saved locally via NSUserDefaults.

## Template Residue Check
- No tw_*, game_01_*, Template, or other template residue found.

## Debug UI
- showsFPS = NO
- showsNodeCount = NO
