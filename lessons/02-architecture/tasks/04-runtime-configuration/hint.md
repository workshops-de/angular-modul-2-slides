<details>
<summary>💡 Provider order</summary>

`provideHttpClient()` must be registered before `provideRuntimeConfiguration()`, because the initializer needs `HttpClient` to fetch the config.

</details>

<details>
<summary>💡 Type safety</summary>

Define an `ApiEndpointsConfiguration` interface and use it for both the JSON response and the signal type to avoid typos and ensure compile-time safety.

</details>
