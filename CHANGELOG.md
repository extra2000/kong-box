# Changelog

## [1.1.0](https://github.com/extra2000/kong-box/compare/v1.0.0...v1.1.0) (2021-03-09)


### Features

* **submodule:** Add [filebeat-formula v1.1.1](https://github.com/extra2000/filebeat-formula/releases/tag/v1.1.1) ([c1dee5c](https://github.com/extra2000/kong-box/commit/c1dee5cbc8844c19a4bf552d4762c566832d27e8))
* **submodule:** Add [zabbix-agent-formula v2.0.1](https://github.com/extra2000/zabbix-agent-formula/releases/tag/v2.0.1) ([d601232](https://github.com/extra2000/kong-box/commit/d60123228b9db2c500181765c981dab2eec31fd4))
* **submodule:** Update `kong-formula` to [v1.0.1](https://github.com/extra2000/kong-formula/releases/tag/v1.0.1) ([435482f](https://github.com/extra2000/kong-box/commit/435482f51a6b7489d6ad2878d4045a39b8072539))


### Code Refactoring

* **submodule:** Remove `cockpit-formula` in favor of `zabbix-agent-formula` ([c574125](https://github.com/extra2000/kong-box/commit/c574125d9a6ef7775f671ed551a8fc58bb251f8b))


### Fixes

* **pillar/kong.sls.example:** Add `bridge` to prevent pod networking conflicts ([f8527fe](https://github.com/extra2000/kong-box/commit/f8527fee3d1382f3faa7a7432f814a1279b7a410))


### Documentations

* **README:** Add instructions to create pillar files for Zabbix agent and Filebeat ([fe4aa12](https://github.com/extra2000/kong-box/commit/fe4aa1263ba496890bf9f364b31c5321f4df6440))
* **README:** Add instructions to deploy Filebeat ([1a60fff](https://github.com/extra2000/kong-box/commit/1a60fffd7a36490fe9ecdd0af0248ee6a8e66c9f))
* **README:** Add Regular Usage instruction to start Filebeat ([a40e42b](https://github.com/extra2000/kong-box/commit/a40e42b4d02bd87e4807e2be1df287db1b274f06))
* **README:** Update Section `Simple Example` ([9ec9880](https://github.com/extra2000/kong-box/commit/9ec9880509e7a3934b99ce8ff77ea8df69870e6a))


### Continuous Integrations

* **AppVeyor:** Add instructions to create Zabbix agent and Filebeat pillar files ([7b7effd](https://github.com/extra2000/kong-box/commit/7b7effd53865a80558bde1b23f1fc21c47de9fc6))

## 1.0.0 (2021-01-24)


### Features

* Add implementations for `salt/` ([c063a54](https://github.com/extra2000/kong-box/commit/c063a541e9493ca2e2077a491ef6a940a048e426))
* **submodule:** Add [cockpit-formula v1.0.3](https://github.com/extra2000/cockpit-formula/releases/tag/v1.0.3) ([2b2ceb9](https://github.com/extra2000/kong-box/commit/2b2ceb9e5b94e66d9074a17eb79e11adc9dd5727))
* **submodule:** Add [kong-formula v1.0.0](https://github.com/extra2000/kong-formula/releases/tag/v1.0.0) ([d919a86](https://github.com/extra2000/kong-box/commit/d919a86fccf88f3a3f06988cfb7d8448e3eab34f))
* **submodule:** Add [podman-formula v2.2.1](https://github.com/extra2000/podman-formula/releases/tag/v2.2.1) ([c60c234](https://github.com/extra2000/kong-box/commit/c60c2342c6c2179505b1d468ad187b8865b6eec0))
* **vagrant:** Import Vagrantfiles from [extra2000/generic-box v1.4.2](https://github.com/extra2000/generic-box/releases/tag/v1.4.2) ([b37ef14](https://github.com/extra2000/kong-box/commit/b37ef14bc10758385aff6db49ce637c7660387b3))


### Continuous Integrations

* Add AppVeyor with `semantic-release` bot ([9a775e5](https://github.com/extra2000/kong-box/commit/9a775e5173543e8258946641a7033af3820d0b10))


### Documentations

* **README:** Update `README.md` ([b428ff1](https://github.com/extra2000/kong-box/commit/b428ff1ff14774becfaa7a9362542bc895faca88))
