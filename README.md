## reprepro

<p align="center"><a href="README.md">English</a> | <a href="README-zh.md">中文</a></p>

This project builds multi-architecture Docker images for [reprepro](https://salsa.debian.org/debian/reprepro) — a tool to manage Debian package repositories, handling `.deb` packages, package pools, and generating repository metadata.

The images are based on **Anolis OS** and support the following architectures:

| Architecture | Platform      |
|--------------|---------------|
| 🐉 loong64   | linux/loong64 |
| 💻 amd64     | linux/amd64   |
| 📱 arm64     | linux/arm64   |

The primary motivation is to bring reprepro to the **LoongArch (loong64)** platform, while also providing amd64 and arm64 variants for convenience.

## Branches and versions

| Branch  | reprepro version | Status          |
|---------|------------------|-----------------|
| `1.2.4` | 1.2.4            | Active          |
| `main`  | —                | Repository root |

The branch name corresponds to the upstream [reprepro](https://salsa.debian.org/debian/reprepro) release version. Tags follow the pattern `<reprepro-version>+<build-number>` (e.g., `1.2.4+2`).

## Docker images

Images are published to Docker Hub under the [`kubernetesloong64/reprepro`](https://hub.docker.com/r/kubernetesloong64/reprepro) namespace.

### Multi-arch manifest

The multi-arch manifest automatically selects the correct architecture for your platform:

| Image tag                                 | Platform                              |
|-------------------------------------------|---------------------------------------|
| `kubernetesloong64/reprepro:1.2.4-anolis` | linux/amd64、linux/arm64、linux/loong64 |

### Architecture-specific images

| Image tag                                         | Platform      |
|---------------------------------------------------|---------------|
| `kubernetesloong64/reprepro:1.2.4-anolis-amd64`   | linux/amd64   |
| `kubernetesloong64/reprepro:1.2.4-anolis-arm64`   | linux/arm64   |
| `kubernetesloong64/reprepro:1.2.4-anolis-loong64` | linux/loong64 |

### Usage

Add `.deb` packages to a Debian repository:

```shell
docker run --rm -v $(pwd)/debs:/debs -v $(pwd)/repo:/repo kubernetesloong64/reprepro:1.2.4-anolis reprepro -b /repo includedeb stable /debs/*.deb
```

Update an existing repository (pull updated packages from upstream):

```shell
docker run --rm -v $(pwd)/repo:/repo kubernetesloong64/reprepro:1.2.4-anolis reprepro -b /repo update stable
```

## Verifying releases

- Releases are signed with GPG.
- Download the public key from [keys.openpgp.org](https://keys.openpgp.org).
- Fingerprint: [FCF8724722CCBF9F51B1FBE376532BE7E3013105](https://keys.openpgp.org/debug?q=FCF8724722CCBF9F51B1FBE376532BE7E3013105)
- [Manual download](https://keys.openpgp.org/vks/v1/by-fingerprint/FCF8724722CCBF9F51B1FBE376532BE7E3013105)

```shell
gpg --keyserver keys.openpgp.org --recv-keys FCF8724722CCBF9F51B1FBE376532BE7E3013105
echo "FCF8724722CCBF9F51B1FBE376532BE7E3013105:6:" | gpg --import-ownertrust
```

Or download the key file manually and import it:

```shell
gpg --import /tmp/xxx
```

## License

[Apache License 2.0](LICENSE)
