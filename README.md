# Video Extension Specification

- **Title:** Video
- **Identifier:** <https://stac-extensions.github.io/video/v1.0.0/schema.json>
- **Field Name Prefix:** video
- **Scope:** Item
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @darrenwiens

This document explains the Video Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

The motivation behind this extension is to provide a standardized way to include various video sources within STAC Items.

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Item Properties

| Field Name           | Type       | Description                                                                                                                                  |
| -------------------- | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| video:shape          | \[integer] | **REQUIRED**. Pixel dimensions of video, expressed as `[rows, columns]`                                                                      |
| video:frame_rate     | number     | The mean frame rate, frames per second (frame count / time extent). Either `video:frame_rate` or `video:frame_count` are highly recommended. |
| video:frame_count    | integer    | The count of frames in the video. Either `video:frame_rate` or `video:frame_count` are highly recommended.                                   |
| video:codec_name     | string     | Four-letter codec code ([list](https://mp4ra.org/#/codecs#))                                                                                 |
| video:constant_scene | boolean    | Flag indicating that both the frame geometries and sensor centers remain constant throughout the video. Default is `false`.                  |
| video:letterbox      | \[integer] | Rectangular window containing useful (non-fill) pixel values, if different from `video:shape`, in the form `[columns_offset, rows_offset, width, height]`                     |

### Assets

The following assets should be included:
| Asset Name | MIME Type | Description |
| -------------------- | -------------------- | -------------------------------------------- |
| video | Asset specific | **REQUIRED**. Path to video asset. |
| video:frame_geometry | application/geo+json | Polygon geometry(s) representing the corners of video frames. This should be a GeoJSON Feature Collection containing: 1 polygon if `video:constant_scene=true`, or n polygons, where `n=video:frame_count` if `video:constant_scene=false`. |
| video:frame_center | application/geo+json | Point geometry(s) representing the center of video frames. This should be a GeoJSON Feature Collection containing: 1 point if `video:constant_scene=true`, or n points, where `n=video:frame_count` if `video:constant_scene=false`. |
| video:sensor_center | application/geo+json | Point geometry(s) representing the location of the sensor. This should be a GeoJSON Feature Collection containing: 1 point if `video:constant_scene=true`, or n points, where `n=video:frame_count` if `video:constant_scene=false` |

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid.
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on
your command line run:

```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:

```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:

```bash
npm run format-examples
```
