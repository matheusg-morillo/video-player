## Testing cases

Available Qualities

|bitrate (bps)|Resolution|
|-----|------|
|5,128,000|1080p|
|2,628,000|720p|
|1,328,000|480p|
|664,000|360p|



### No throttling
- The selected quality was 360 unexpectedly. video.js uses the lowest quality first, and since locally the load time is instant, the bandwidth cannot be calculated properly.

```js
const bandwidth = bytesReceived / download_time
```

- Since download_time is almost 0, `bytes / 0` = Infinity or NaN, vhs probably catches this error and defaults to 0, causing the lowest quality to be selected.

### Throttling 1mbs
- selected quality was 360p, as expected, as `system bandwidth = 1050000 bps` and cannot load 480 as it needs at least 1,328,000 bps.

### Throttling 2mbs
- selected quality was 360p, unexpectedly. It should be able to load 480p as `system bandwidth = 2,100,000 bps` which is greater than 1,328,000 bps. However vhs needs more headroom to comfortably load a quality, so it sticks to 360p.


### Throttling 3mbs
- selected quality was 480p, as expected. Since it can comfortably afford 480p with `system bandwidth = 3,150,000 bps` which is greater than 1,328,000 bps, with a headroom aprox. 50%.
