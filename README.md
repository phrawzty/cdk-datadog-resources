# AWS CDK Datadog Resources

[![npm version](https://badge.fury.io/js/%40nomadblacky%2Fcdk-datadog-resources.svg)](https://badge.fury.io/js/%40nomadblacky%2Fcdk-datadog-resources)

An AWS CDK construct library that wrapped [DataDog/datadog-cloudformation-resources](https://github.com/DataDog/datadog-cloudformation-resources).

## Requirements

Before use this library, [register datadog-cloudformation-resources to your AWS account.](https://github.com/DataDog/datadog-cloudformation-resources#datadog-aws-cloudformation)

You need to register the correct version listed in `Supported Resources`.

## Supported CDK Languages

- TypeScript
- Python
- ~~Java~~ Sorry, there is a problem with the release. ([#22](https://github.com/NomadBlacky/cdk-datadog-resources/issues/22))

## Supported Resources

| Supported? | Resource                | Datadog CF Resource Name         | Description                                              | Datadog CF Version |
| :--------: | ----------------------- | -------------------------------- | -------------------------------------------------------- | ------------------ |
|     ✅     | Dashboards              | `Datadog::Dashboards::Dashboard` | [Create, update, and delete Datadog dashboards.][1]      | [1.0.0][7]         |
|            | Datadog-AWS integration | `Datadog::Integrations::AWS`     | [Manage your Datadog-Amazon Web Service integration.][2] | N/A                |
|     ✅     | Monitors                | `Datadog::Monitors::Monitor`     | [Create, update, and delete Datadog monitors.][3]        | [3.0.0][6]         |
|            | Downtimes               | `Datadog::Monitors::Downtime`    | [Enable or disable downtimes for your monitors.][4]      | N/A                |
|            | User                    | `Datadog::IAM::User`             | [ Create and manage Datadog users.][5]                   | N/A                |

## Installation

TypeScript

```shell
npm install @nomadblacky/cdk-datadog-resources
```

Python

```shell
pip install cdk-datadog-resources
```

Java

```xml
<dependency>
    <groupId>dev.nomadblacky</groupId>
    <artifactId>cdk-datadog-resources</artifactId>
    <version>x.y.z</version>
</dependency>
```

## Usage

Belows are examples of TypeScript.

### Dashboards

```typescript
import * as fs from 'fs';
import { DatadogDashboard } from '@nomadblacky/cdk-datadog-resources';

new DatadogDashboard(yourStack, 'TestDashboard', {
  datadogCredentials: {
    apiKey: process.env.DATADOG_API_KEY!,
    applicationKey: process.env.DATADOG_APP_KEY!,
  },
  dashboardDefinition: fs.readFileSync(`${__dirname}/path/to/your/dashboard-definition.json`).toString(),
});
```

### Monitors

```typescript
import { DatadogMonitor } from '@nomadblacky/cdk-datadog-resources';

new DatadogMonitor(yourStack, 'TestMonitor', {
  datadogCredentials: {
    apiKey: process.env.DATADOG_API_KEY!,
    applicationKey: process.env.DATADOG_APP_KEY!,
  },
  query: 'avg(last_1h):sum:system.cpu.system{host:host0} > 100',
  type: MonitorType.QueryAlert,
  name: 'Test Monitor',
  options: {
    thresholds: {
      critical: 100,
      warning: 80,
      oK: 90,
    },
    notifyNoData: true,
    evaluationDelay: 60,
  },
});
```

[1]: https://github.com/DataDog/datadog-cloudformation-resources/tree/master/datadog-dashboards-dashboard-handler
[2]: https://github.com/DataDog/datadog-cloudformation-resources/tree/master/datadog-integrations-aws-handler
[3]: https://github.com/DataDog/datadog-cloudformation-resources/tree/master/datadog-monitors-monitor-handler
[4]: https://github.com/DataDog/datadog-cloudformation-resources/tree/master/datadog-monitors-downtime-handler
[5]: https://github.com/DataDog/datadog-cloudformation-resources/tree/master/datadog-iam-user-handler
[6]: https://github.com/DataDog/datadog-cloudformation-resources/blob/master/datadog-monitors-monitor-handler/CHANGELOG.md#300--2021-02-16
[7]: https://github.com/DataDog/datadog-cloudformation-resources/blob/master/datadog-dashboards-dashboard-handler/CHANGELOG.md#100--2021-02-16
