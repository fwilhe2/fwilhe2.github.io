# Using Kotlin as a Shell for GitHub Actions workflows

Published 2021-01-24 by [fwilhe](https://social.tchncs.de/@fwilhe)

One interesting thing about GitHub Actions is that you can chose a ["custom shell"](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#custom-shell) per step.
In connection with Kotlin's [scripting capabilities](https://github.com/Kotlin/kotlin-script-examples/blob/master/jvm/main-kts/MainKts.md), this allows you to write "shell" scripts in Kotlin.

Here is a trivial example

```yaml
  build:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
      - uses: fwilhe2/setup-kotlin@main
        with:
          version: 1.4.30-RC
      - run: |
            java.io.File(".").listFiles().forEach {it -> println(it.getName().toString())}
        shell: kotlin -howtorun .main.kts {0}
```

The [`fwilhe2/setup-kotlin`](https://github.com/fwilhe2/setup-kotlin) action is used here to setup the Kotlin cli which is required to use it as a scripting interpreter as it is not installed by default on GitHub's virtual environments.

The `howtorun` flag is new in Kotlin `1.4.30-RC` and is required because [GitHub does not allow to set a custom file extension](https://github.com/actions/runner/issues/813).

"Why?", you might ask at this point.
Well, maybe you're more proficient in writing Kotlin compared to Bash or Python?
I did not use this very much so far, but I'm eager to see in which cases this might be useful.
And, last but not least, finding this out led me down a rabbit hole of how GitHub Actions and the Kotlin cli work, so that was interesting.
