## Calibrate a prediction interval

Use examples held out from model training. Each interval is prediction ± a finite calibration radius. Coverage is marginal under exchangeability, and can differ by subgroup or after a distribution shift. A 90% request needs at least nine calibration examples for a finite interval.

1. Open an empty file named `conformal-demo.blyx`.
2. Type `blyx-conformal-demo` and select **Conformal — runnable example**. You can also use **Snippets: Fill File with Snippet**.
3. Choose **Blyx: Run File** from the command palette. Change an argument near the end of `fn main()` to try another input.

Expected output

```text
2
18
22
```

[Work through the example in the browser](https://www.blyx-lang.space/play?example=package-conformal)
