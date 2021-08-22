# Feast Custom Provider

### Overview

This repository demonstrates how developers can create their own custom `providers` for Feast. Custom providers can be
used like plugins and allow Feast users to execute any custom logic. Typical examples include
* Launching custom Spark streaming ingestion jobs
* Launching custom batch ingestion (materialization) jobs
* Adding custom validation to feature repositories during `apply`
* Adding custom infrastructure setup logic which runs during `apply`
* Extending Feast commands with in-house metrics, logging, or tracing

### Extending this provider
All Feast operations execute through a provider. Operations like materializing data from the offline to the online
store, updating infrastructure like databases, launching streaming ingestion jobs, building training datasets, and
reading features from the online store.

Feast comes with providers built in, e.g, LocalProvider, GcpProvider, and AwsProvider. However, users can develop their
own providers by creating a class that implements the contract in the [Provider class](https://github.com/feast-dev/feast/blob/745a1b43d20c0169b675b1f28039854205fb8180/sdk/python/feast/infra/provider.py#L22).

Most developers, however, simply want to add new logic to Feast and don't necessarily want to create a whole provider on
their own. In that case, the simplest way to add custom logic to Feast is to extend a provider. The most generic
provider is the LocalProvider, which contains no custom logic specific to a cloud environment.

This repository contains an example of a custom provider, `MyCustomProvider`, which simply extends the Feast
`LocalProvider`.

### Testing the provider

Run the following commands to test the provider

```bash
pip install -r requirements.txt
```

```
pytest test_custom_provider.py
```

It is also possible to run Feast CLI command, which in turn will call the provider. It may be necessary to add the 
`PYTHONPATH` to the path where your provider module is stored.
```
PYTHONPATH=$PYTHONPATH:/$(pwd) feast -c basic_feature_repo apply
```
```
Registered entity driver_id
Registered feature view driver_hourly_stats
Deploying infrastructure for driver_hourly_stats
Launching custom streaming jobs is pretty easy...
```