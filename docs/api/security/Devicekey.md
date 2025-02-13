# DeviceKey

<span class="images">![](https://os.mbed.com/docs/development/mbed-os-api-doxy/classmbed_1_1_device_key.png)<span>DeviceKey class hierarchy</span></span>

DeviceKey is a mechanism that implements key derivation from a root of trust key. The DeviceKey mechanism generates symmetric keys that security features need. You can use these keys for encryption, authentication and more. The DeviceKey API allows key derivation without exposing the actual root of trust, to reduce the possibility of accidental exposure of the root of trust outside the device.

We have implemented DeviceKey according to NIST SP 800-108, section "KDF in Counter Mode", with AES-CMAC as the pseudorandom function.

## Root of Trust

The root of trust key, which DeviceKey uses to derive additional keys, is generated using the hardware random generator if it exists, or using a key injected to the device in the production process.

The characteristics required by this root of trust are:

- It must be unique per device.
- It must be difficult to guess.
- It must be at least 128 bits.
- It must be kept secret.

The DeviceKey feature keeps the root of trust key in internal storage, using the KVStore component. Internal storage provides protection from external physical attacks to the device.

The root of trust is generated at the first use of DeviceKey if the true random number generator is available in the device. If no true random number generator is available, you must pass the injected root of trust key to the DeviceKey before you call the key derivation API.

## Key derivation API

`generate_derived_key`: This API generates a new key based on an array of data ([salt](https://en.wikipedia.org/wiki/Salt_(cryptography)) the caller provides. A single salt value always generates the same key, so if you need a new key, you must use a new salt value. The salt can have any value - array, string and so on.

The generated keys can be 128 or 256 bits in length.

### Root of Trust Injection API

`device_inject_root_of_trust`: You must call this API once in the lifecycle of the device, before any call to key derivation, if the device does not support true random number generator (`DEVICE_TRNG` is not defined).

### Using DeviceKey

DeviceKey is a singleton class, meaning that the system can have only a single instance of it.

To instantiate DeviceKey, you need to call its `get_instance` member function as following:

```c++ TODO
    DeviceKey &deviceKey = DeviceKey::get_instance();
```

### Testing DeviceKey

Run the DeviceKey functionality test with the `mbed` command as following:

```
    mbed test -n features-device_key-tests-device_key-functionality
```

## DeviceKey API class reference

[![View code](https://www.mbed.com/embed/?type=library)](https://os.mbed.com/docs/development/mbed-os-api-doxy/classmbed_1_1_device_key.html)

## DeviceKey example

[![View Example](https://www.mbed.com/embed/?url=https://github.com/ARMmbed/mbed-os-examples-docs_only/blob/master/DeviceKey)](https://github.com/ARMmbed/mbed-os-examples-docs_only/blob/master/DeviceKey/main.cpp)
