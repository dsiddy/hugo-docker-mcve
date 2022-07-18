# `hugo-docker-mcve`

An [MCVE](https://stackoverflow.com/help/minimal-reproducible-example) to illustrate my own encounter with [`klakegg/docker-hugo#61`](https://github.com/klakegg/docker-hugo/issues/61).

(Thanks, `klakegg`, for your Hugo image, and for helping me to resolve this issue. 😊)

## reproduction steps

1. Run `docker compose up -d` to spin up a Docker container.
2. Open `http://localhost:1313`.
3. Observe the content of `src/content/posts/my-test-post.md`.
4. Change the content of `src/content/posts/my-test-post.md` from the host machine.
5. Observe that the page doesn't auto-reload with the new content, and that a manual refresh also fails to update the content.

Note that changing the content from _within_ the container also fails to trigger a reload. Also, changing `command: server` to `command: server --watch` in `compose.yaml` doesn't help, nor does adding any of the other flags suggested in the issue comments.

## creation steps

Apart from initializing a Git repository and writing `compose.yaml` and this `README`, I followed the [quick start guide](https://gohugo.io/getting-started/quick-start/):

1. `hugo new site src`
2. `cd src`
3. `git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke`
4. `echo theme = \"ananke\" >> config.toml` (I did this from within a Kali Linux instance running under WSL 2, because PowerShell's `echo` wrote additional binary content to `config.toml`.)
5. `hugo new posts/my-test-post.md`

I then appended the following to `src/content/posts/my-test-post.md`:

```html
Hello, world!
<!-- 'Til next time, world! -->
```

Finally, I set `draft: false` in the above post's front matter.

## system information


### Hugo

```
> hugo version
hugo v0.101.0-466fa43c16709b4483689930a4f9ac8add5c9f66+extended windows/amd64 BuildDate=2022-06-16T07:09:16Z VendorInfo=gohugoio
```

### Docker

```
> docker version
Client:
 Cloud integration: v1.0.24
 Version:           20.10.17
 API version:       1.41
 Go version:        go1.17.11
 Git commit:        100c701
 Built:             Mon Jun  6 23:09:02 2022
 OS/Arch:           windows/amd64
 Context:           default
 Experimental:      true

Server: Docker Desktop 4.10.1 (82475)
 Engine:
  Version:          20.10.17
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.17.11
  Git commit:       a89b842
  Built:            Mon Jun  6 23:01:23 2022
  OS/Arch:          linux/amd64
  Experimental:     true
 containerd:
  Version:          1.6.6
  GitCommit:        10c12954828e7c7c9b6e0ea9b0c02b01407d3ae1
 runc:
  Version:          1.1.2
  GitCommit:        v1.1.2-0-ga916309
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

## to do

- Package this whole MCVE in its own Docker container. This'll require using Docker wihin Docker, which is [possible](https://hub.docker.com/_/docker) (though I've never tried it).
