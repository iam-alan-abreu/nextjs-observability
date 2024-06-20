
# nextjs-observability


The Next.js app included with the template has been generated with [`create-next-app`](https://nextjs.org/docs/getting-started/installation#automatic-installation) and has some additional code and components specific to this template that provide:

* [Server-side and client-side instrumentation and logging via App Insights](#application-insights)

## Quickstart

Then from a Terminal:

```bash
# install dependencies
npm i

# run locally
npm run dev

```




## Application Insights

The template includes instrumentation and components to enable server-side and client-side instrumentation and logging via App Insights when deployed to Azure.

### Server-side instrumentation and components

Server-side instrumentation is implemented using the [Application Insights for Node.js (Beta)](https://github.com/microsoft/ApplicationInsights-node.js/tree/beta) package. The package is initialised via Next.js's [Instrumentation](https://nextjs.org/docs/app/building-your-application/optimizing/instrumentation) feature and leverages Next.js's support for [OpenTelemetry](https://nextjs.org/docs/app/building-your-application/optimizing/open-telemetry).

The template also provides a logging implementation to allow for explicit logging of errors, warnings, etc., in your app's server-side code (including inside server components). The logger is implemented using [`pino`](https://getpino.io/) and sends logs to Application Insights via a `pino` [transport](https://github.com/CMeeg/pino-appinsights-transport).

💡 To prevent runtime errors the `applicationinsights` and `pino` packages are opted-out of Next.js's bundling process via [`serverComponentsExternalPackages`](https://nextjs.org/docs/app/api-reference/next-config-js/serverComponentsExternalPackages).

> Server-side instrumentation and logging to Application Insights requires a connection string to an Application Insights resource to be provided via the environment variable `APPLICATIONINSIGHTS_CONNECTION_STRING`. This environment variable is provided to the app automatically by `azd provision`.
>
> If the connection string is not available, for example, when running in your development environment outside of `azd provision`, the instrumentation will not be initialised and the logger will fallback to using the [`pino-pretty`](https://github.com/pinojs/pino-pretty) transport to log to `console`.

### Client-side instrumentation and logging

Client-side instrumentation is implemented using the [Microsoft Application Insights JavaScript SDK - React Plugin](https://github.com/microsoft/applicationinsights-react-js) package. The package is initialised in a `AppInsightsProvider` server component, which is a wrapper around a client component that renders the `AppInsightsContext` component from the React Plugin. You can `import { AppInsightsProvider } from '@/components/instrumentation/AppInsightsProvider'` and place it somewhere fairly high up in the component tree, for example, this template renders the component inside its [Root Layout](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#root-layout-required).

Client-side logging can be performed by using the `useAppInsightsContext` hook from within client components as described in the [documentation for the React Plugin](https://learn.microsoft.com/en-gb/azure/azure-monitor/app/javascript-framework-extensions?tabs=react#use-application-insights-with-react-context).

> Client-side instrumentation and logging to Application Insights requires a connection string to an Application Insights resource to be provided via the environment variable `APPLICATIONINSIGHTS_CONNECTION_STRING`. This environment variable is provided to the app automatically by `azd provision`.
>
> If the connection string is not available, for example, when running in your development environment outside of `azd provision`, the `AppInsightsProvider` will not render and `useAppInsightsContext` will return `undefined`.

🙏 Thank you to [Jonathan Rupp](https://github.com/jorupp) who very kindly shared his implementation for client-side instrumentation in this [GitHub discussion](https://github.com/vercel/next.js/discussions/55405#discussioncomment-7118671), which was mostly reused for the implementation in this template.

## Environment variables

When developing your app you should use environment variables as per the [Next documentation](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables):

* Use `.env` for default vars for all builds and environments
