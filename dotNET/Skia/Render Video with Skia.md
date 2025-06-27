```bash
dotnet add package SkiaSharp
dotnet add package FFMpegCore
dotnet add package FFMpegCore.Extensions.SkiaSharp
```

```cs
using FFMpegCore;
using FFMpegCore.Pipes;
using SkiaSharp;
```

Draw frames
```cs
using SKBitmap bmp = new(width, height);
using SKCanvas canvas = new(bmp);
canvas.Clear(SKColors.White);

// draw to canvas

using BitmapVideoFrameWrapper frame = new(bmp);
yield return (IVideoFrame)frame;
```

Use `IEnumerable<IVideoFrame> frames` to create video:

```cs
RawVideoPipeSource videoFramesSource = new(frames) { FrameRate = 30 };
bool success = FFMpegArguments
    .FromPipeInput(videoFramesSource)
    .OutputToFile("output.webm", overwrite: true, options => options.WithVideoCodec("libvpx-vp9"))
    .ProcessSynchronously();
```

## Codec to use

| Codec        | File Extension      | Use case               |
| ------------ | ------------------ | ---------------------- |
| `mpeg4`      | `*.mp4`, `*.mpeg4` | Media Player           |
| `libx264`    | `*.mp4`, `*.mpeg4` | Browser                |
| `libvpx-vp9` | `*.webm`           | Browser & Media Player |

## Sources

- [Render Video with SkiaSharp](https://swharden.com/csdv/skiasharp/video/)
