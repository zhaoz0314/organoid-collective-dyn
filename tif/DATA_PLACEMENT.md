```text
tif/
├── cab-<network>-<recording>-<condition>.tif
└── org-<source-folder>-<preparation>-<condition>.tif
```

The TIFF files sit directly in `tif/`, without an additional directory level.

For BTO recordings, `11` and `13` preserve the labels of the source directories in which the data were received:

```text
C5D_7978_samp11_TD150_TIFF8bit/
C5D_RDH913_samp13_TD154_TIFF8bit/
```

The first numeric component preserves the source folder in which the data were received. The following numeric component distinguishes individual preparations within that folder.
