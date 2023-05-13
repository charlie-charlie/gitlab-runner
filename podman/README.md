podman and bunch of other sw must be installed(rhl 8 included)
install/register gitlab-runner same
sudo systemctl enable podman.socket
 sudo systemctl start podman.socket
sudo systemctl status  podman.socket 

[root@perf-ocp-ans01 ~]# systemctl status podman.socket
● podman.socket - Podman API Socket
   Loaded: loaded (/usr/lib/systemd/system/podman.socket; enabled; vendor preset: disabled)
   Active: active (listening) since Tue 2022-11-15 10:53:43 EST; 3h 43min ago
     Docs: man:podman-system-service(1)
   Listen: /run/podman/podman.sock (Stream)
    Tasks: 0 (limit: 23516)
   Memory: 0B
   CGroup: /system.slice/podman.socket
 
Nov 15 10:53:43 perf-ocp-ans01 systemd[1]: Listening on Podman API Socket.
the important part is Listen: line

vi /etc/gitlab-runner/config.toml, add host = line from above output

[root@perf-ocp-ans01 ~]# more /etc/gitlab-runner/config.toml
concurrent = 1
check_interval = 0
 
[session_server]
  session_timeout = 1800
 
[[runners]]
  name = "perf-ocp-ans-01 at vcenter perflab-vc01"
  url = "https://gitlab.otxlab.net/"
  proxy = "http://bp2-prox01-l001.otxlab.net:3128"
  id = 4461
  token = "sApC8jiG2JNC4ok4Mys-"
  token_obtained_at = 2022-09-01T16:06:36Z
  token_expires_at = 0001-01-01T00:00:00Z
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    host = "unix:///run/podman/podman.sock"
    image = "gitlab/gitlab-runner:latest"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0
restart gitlab-runner
