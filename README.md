# WES GPS Publisher

This project creates a docker image (intended to be used in k3s) that accesses the hosts GPS device and publishes the GPS data on the containers 2974 port (that needs to be explicitly exposed at runtime).

## Accessing the GSP Data from the Host

It is possible to access the GPS data published by this image via the host by using the IP address of the container.  The
following instructions assume that the container is running as a kubernetes pod with port 2974 exposed.

First identify the IP address of either the GPS pod or service

```
# kubectl get pod -o wide | grep gps
wes-gps-server-8459c5999d-hbp8h                        1/1     Running            0          2m16s   10.42.0.14   000048b02d15bc68.ws-nxcore   <none>           <none>
```

or

```
# kubectl get service wes-gps-server
NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
wes-gps-server   ClusterIP   10.43.158.62   <none>        2947/TCP   3m7s
```

Now on the host execute `gpsdclient` (or any other GPS client) specifying the host IP address

```
# gpsdclient --host 10.43.158.62
Connected to gpsd v3.17
Devices: /host/dev/gps

Mode | Time                 | Lat          | Lon          | Track  | Speed  | Alt       | Climb
-----+----------------------+--------------+--------------+--------+--------+-----------+----------
3    | 2021-10-05 15:01:37  | 41.786593275 | -88.013486412 | 75.5553 | 0.26   | 232.281   | 0.003
3    | 2021-10-05 15:01:37  | 41.786593167 | -88.013486333 | 0.0    | 0.26   | 231.8     | n/a
3    | 2021-10-05 15:01:37  | 41.786593167 | -88.013486333 | 0.0    | 0.26   | 231.8     | 0.0
3    | 2021-10-05 15:01:38  | 41.786597342 | -88.013488185 | 30.1954 | 0.249  | 231.806   | -0.016
3    | 2021-10-05 15:01:38  | 41.786597333 | -88.013488167 | 0.0    | 0.246  | 231.3     | n/a
```

