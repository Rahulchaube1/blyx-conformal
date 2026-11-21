<img src="assets/logo.png" width="88" height="88" alt="Blyx Conformal logo">

# Blyx Conformal

Calibrate a regression interval from held-out errors and measure coverage on a separate test set. Take a fixed regression model’s errors on a calibration set, then attach an interval to each new prediction. The calibration rank uses exact integer arithmetic. Selection works over the input vectors without sorting or allocating a second score array.

## Use it in a project

Install [Blyx 0.2.1](https://github.com/Rahulchaube1/blyxxxx/releases/tag/v0.2.1), Rust 1.85 or newer, and a host linker for native compilation. From the directory containing your entry file, pin the library with Git.

```sh
git clone --depth 1 --branch v0.1.0 https://github.com/Rahulchaube1/blyx-conformal.git vendor/conformal
```

Save this program as `main.blyx` beside `vendor` and run `blyxc native-run main.blyx`.

```blyx
use vendor::conformal::conformal;
fn main() {
    let calibration = cf_calibrate([10, 12, 7, 11], [10, 10, 10, 10], 3, 5);
    let interval = cf_interval(20.0, at(calibration, 0));
    println!(at(calibration, 0));
    println!(at(interval, 0));
    println!(at(interval, 1));
}
```

Expected output:

```text
2
18
22
```

[Run this example in your browser](https://www.blyx-lang.space/play?example=package-conformal)

## API and data contract

| Function | Result |
| --- | --- |
| `cf_calibrate(actual, predicted, numerator, denominator)` | Compute absolute residuals and select [radius, one-based rank, sample count]. |
| `cf_radius(scores, numerator, denominator)` | Select the same result from finite nonnegative scores. |
| `cf_rank(n, numerator, denominator)` | ceil((n+1) × target_coverage), or 0 for an invalid request. |
| `cf_interval(prediction, radius)` | Lower and upper endpoints, inclusive. |
| `cf_coverage(actual, predicted, radius)` | Fraction covered on a separate test set, or −1 for invalid data. |

Pass coverage as a fraction such as 9/10. The denominator is at most 1,000,000 and the numerator must lie strictly between zero and the denominator. The selected radius is an observed score, including when scores tie. An empty calibration result means the request is invalid, numeric limits were exceeded, or the requested coverage requires a rank above the available data. For 90% coverage at least nine calibration rows are necessary to get a finite radius.

Fit the model on separate training data. The standard finite-sample marginal coverage result requires exchangeability between calibration and future examples. Subgroups and time-shifted data can have different coverage. Selection uses constant auxiliary storage and can rescan the scores several times, depending on their range. [Conformal prediction tutorial and assumptions](https://arxiv.org/abs/2107.07511)

## Contribute

Clone this repository, run `blyxc native-run tests.blyx` and `blyxc native-run example.blyx`, and include a failing example with any behavior change. Include the compiler version, inputs and expected output in bug reports. [Open a package issue](https://github.com/Rahulchaube1/blyx-conformal/issues) · [Blyx handbook](https://doc.blyx-lang.space/guide/training-and-monitoring) · [Ecosystem catalog](https://www.blyx-lang.space/packages)

## Connect with Rahul Chaube

[Portfolio](https://www.rahulchaube1.com/) · [GitHub](https://github.com/Rahulchaube1) · [LinkedIn](https://www.linkedin.com/in/rahulchaube1) · [Hugging Face](https://huggingface.co/oncody) · [X](https://x.com/RahulChaube_) · [EverestQ](https://www.everestqai.com/) · [Contact](https://www.rahulchaube1.com/contact)

[Blyx website](https://www.blyx-lang.space/) · [BlyxTorch](https://www.blyx-lang.space/packages/blyxtorch) · [Handbook](https://doc.blyx-lang.space/) · [Community](https://www.blyx-lang.space/community)

Support Rahul’s open-source work through [GitHub Sponsors](https://github.com/sponsors/Rahulchaube1) or [Buy Me a Coffee](https://buymeacoffee.com/rahulchaube).
