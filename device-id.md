The DeviceId gRPC service provides device identification details for a given IP address.

This module requires membership in the 'deviceid' API access group.

Here is a sample client written in Python:

```
import grpc
import device_id_pb2
import device_id_pb2_grpc
import sys

def run():
    with grpc.secure_channel('grpc.shadowserver.org:443',
            grpc.composite_channel_credentials(
                grpc.ssl_channel_credentials(),
                grpc.access_token_call_credentials('my-api-key')
                )
        ) as channel:
        stub = device_id_pb2_grpc.DeviceIdStub(channel)
        try:
            device = stub.Lookup(device_id_pb2.Request(ip=sys.argv[1]))
            print(device)
        except grpc.RpcError as e:
            print("Error %r" % e.code())

if __name__ == '__main__':
    run()
```

Sample usage:

```
python3 device_id_client.py 192.168.1.1
ip: "192.168.1.1"
device_type: "cast-device"
device_sector: "consumer"

```
