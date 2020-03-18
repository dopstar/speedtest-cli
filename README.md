![LibreSpeed Logo](https://github.com/librespeed/speedtest/blob/master/.logo/logo3.png?raw=true)

# LibreSpeed command line tool
Don't have a GUI but wants to use LibreSpeed servers to test your Internet speed? 🚀

`librespeed-cli` comes to rescue!

This is a command line interface for LibreSpeed speed test backends, written in Go.

## Features
- Ping
- Jitter
- Download
- Upload
- IP address
- ISP Information
- Result sharing (telemetry) *[optional]*
- Test with multiple servers in a single run
- Use your own server list or telemetry endpoints
- Tested with PHP and Go backends

[![asciicast](https://asciinema.org/a/J17bUAilWI3qR12JyhfGvPwu2.svg)](https://asciinema.org/a/J17bUAilWI3qR12JyhfGvPwu2)

## Requirements for compiling
- Go 1.14

## Runtime requirements
- Any [Go supported platforms](https://github.com/golang/go/wiki/MinimumRequirements)

## Building `librespeed-cli`

1. First, you'll have to install Go. For Windows users, [you can download an installer from golang.org](https://golang.org/dl/).
For Linux users, you can use either the archive from golang.org, or install from your distribution's package manager.

    For example, Arch Linux:

    ```shell script
    # pacman -S go
    ```
   
2. Then, clone the repository:

    ```shell script
    $ git clone -b v1.0.0 https://github.com/librespeed/speedtest-cli
    ```

3. After you have Go installed on your system (and added to your `$PATH` if you're using the archive from golang.org), you
can now proceed to build `librespeed-cli` with the build script:

    ```shell script
    $ cd speedtest-cli
    $ ./build.sh
    ```

    If you want to build for another operating system or system architecture, use the `GOOS` and `GOARCH` environment
    variables. Run `go tool dist list` to get a list of possible combinations of `GOOS` and `GOARCH`.
    
    ```shell script
    # Let's say we're building for 64-bit Windows on Linux
    $ GOOS=windows GOARCH=amd64 ./build.sh
    ```

4. When the build script finishes, if everything went smoothly, you can find the built binary under directory `out`.

    ```shell script
    $ ls out
    librespeed-cli-windows-amd64.exe
    ```
   
5. Now you can use the `librespeed-cli` and test your Internet speed!

## Install from AUR

To install `librespeed-cli` from AUR, use your favorite AUR helper and install package `librespeed-cli-bin`

```shell script
$ yay librespeed-cli-bin
```

... or, clone it and build it yourself:

```shell script
$ git clone https://aur.archlinux.org/librespeed-cli-bin.git
$ cd librespeed-cli-bin
$ makepkg -si
```

## Install from Homebrew

See the [librespeed-cli Homebrew tap](https://github.com/librespeed/homebrew-tap#setup).

## Usage

You can see the full list of supported options with `librespeed-cli -h`:

```shell script
$ librespeed-cli -h
NAME:
   librespeed-cli - Test your Internet speed with LibreSpeed 🚀

USAGE:
   librespeed-cli [global options] [arguments...]

GLOBAL OPTIONS:
   --help, -h                     show help (default: false)
   --version                      Show the version number and exit (default: false)
   --ipv4, -4                     Force IPv4 only (default: false)
   --ipv6, -6                     Force IPv6 only (default: false)
   --no-download                  Do not perform download test (default: false)
   --no-upload                    Do not perform upload test (default: false)
   --concurrent value             Concurrent HTTP requests being made (default: 3)
   --bytes                        Display values in bytes instead of bits. Does not affect
                                  the image generated by --share, nor output from
                                  --json or --csv (default: false)
   --mebibytes                    Use 1024 bytes as 1 kilobyte instead of 1000 (default: false)
   --distance value               Change distance unit shown in ISP info, use 'mi' for miles,
                                  'km' for kilometres, 'NM' for nautical miles (default: "km")
   --share                        Generate and provide a URL to the LibreSpeed.org share results
                                  image, not displayed with --csv (default: false)
   --simple                       Suppress verbose output, only show basic information
                                   (default: false)
   --csv                          Suppress verbose output, only show basic information in CSV
                                  format. Speeds listed in bit/s and not affected by --bytes
                                   (default: false)
   --csv-delimiter CSV_DELIMITER  Single character delimiter (CSV_DELIMITER) to use in
                                  CSV output. (default: ",")
   --csv-header                   Print CSV headers (default: false)
   --json                         Suppress verbose output, only show basic information
                                  in JSON format. Speeds listed in bit/s and not
                                   affected by --bytes (default: false)
   --list                         Display a list of LibreSpeed.org servers (default: false)
   --server SERVER                Specify a SERVER ID to test against. Can be supplied
                                  multiple times. Cannot be used with --exclude
   --exclude EXCLUDE              EXCLUDE a server from selection. Can be supplied
                                  multiple times. Cannot be used with --server
   --server-json value            Use an alternative server list from remote JSON file
   --local-json value             Use an alternative server list from local JSON file
   --source SOURCE                SOURCE IP address to bind to
   --timeout TIMEOUT              HTTP TIMEOUT in seconds. (default: 15)
   --secure                       Use HTTPS instead of HTTP when communicating with
                                  LibreSpeed.org operated servers (default: false)
   --no-pre-allocate              Do not pre allocate upload data. Pre allocation is
                                  enabled by default to improve upload performance. To
                                  support systems with insufficient memory, use this
                                  option to avoid out of memory errors (default: false)
   --telemetry-json value         Load telemetry server settings from a JSON file. This
                                  options overrides --telemetry-level, --telemetry-server,
                                  --telemetry-path, and --telemetry-share. Implies --share
   --telemetry-level value        Set telemetry data verbosity, available values are:
                                  disabled, basic, full, debug. Implies --share
   --telemetry-server value       Set the telemetry server base URL. Implies --share
   --telemetry-path value         Set the telemetry upload path. Implies --share
   --telemetry-share value        Set the telemetry share link path. Implies --share
   --telemetry-extra value        Send a custom message along with the telemetry results.
                                  Implies --share
```

## Use a custom backend server list
The `librespeed-cli` supports loading custom backend server list from a JSON file (remotely via `--server-json` or
locally via `--local-json`). The format is as below:

```json
[
  {
    "name": "PHP Backend",
    "server": "https://example.com/",
    "dlURL": "garbage.php",
    "ulURL": "empty.php",
    "pingURL": "empty.php",
    "getIpURL": "getIP.php"
  },
  {
    "name": "Go Backend",
    "server": "http://example.com/speedtest/",
    "dlURL": "garbage",
    "ulURL": "empty",
    "pingURL": "empty",
    "getIpURL": "getIP"
  }
]
```

As you can see in the example, all servers have their schemes defined. In case of undefined scheme (e.g. `//example.com`),
`librespeed-cli` will use `http` by default, or `https` when the `--secure` option is enabled.

## Use a custom telemetry server
By default, the telemetry result will be sent to `librespeed.org` by default. You can also customize your telemetry settings 
via the `--telemetry` prefixed options. In order to load a custom telemetry endpoint configuration, you'll have to use the
`--telemetry-json` option to specify a local JSON file containing the configuration bits. The format is as below:

```json
{
  "telemetryLevel": "full",
  "server": "https://example.com",
  "path": "/results/telemetry.php",
  "shareURL": "/results/"
}
```

For `telemetryLevel`, four values are available:

- `disabled` to disable telemetry
- `basic` to enable telemetry with result only)
- `full` to enable telemetry with result and timing
- `debug` to enable the most verbose telemetry information

`server` defines the base URL for the backend's endpoints. `path` is the path for uploading the telemetry result, and
`shareURL` is the path for fetching the uploaded result in PNG format.

Currently, `--telemetry-json` only supports loading a local JSON file.

## Bugs?

Although we have tested the cli, it's still in its early days. Please open an issue if you encounter any bugs, or even
better, submit a PR.

## How to contribute

If you have some good ideas on improving `librespeed-cli`, you can always submit a PR via GitHub.

## License

`librespeed-cli` is licensed under [GNU Lesser General Public License v3.0](https://github.com/librespeed/speedtest-cli/blob/master/LICENSE)
