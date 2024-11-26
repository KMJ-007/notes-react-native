---
title: How to handle env in project?
tags:
  - env
  - good-practice
  - typescript
---
After working on many projects i have adopted this good env handling pattern in general.

my almost all projects have some kind of env, and many times i forgot to create env file or end up forgetting to update env that other team member has updated, and i don't realised that untill the project doesn't crash or something breaks, which is kind of not good pattern in my pov, humans will forget and always make mistake, one should design system in such way that it minimises the known/unknown mistakes.

and how i have seen people handle env in that also many time they end up mixing values of different var, or the logic becomes really complicated to handle different `production`,`development`, `staging` environment envs, and when working in team where new vars and logic is changing cause that's the need and you pull the latest changes and now you are confused as hell why things are not working on!

her's how i handle my env in my projects and big team projects:

>[!TIP]
>Use typescript and some kind of validation lib to make your and others life easy and save time, in starting it will suck but in long term amount of mistakes and roi is really worth the pain in small/large team.

#### central env config:

I use [zod](https://zod.dev/) for validation and getting strict type checking, type inference and autocomplete across my whole app.

```typescript
// env.config.ts

import { z } from "zod";
import "dotenv/config";

// dump all the fields which are there
const defaultFields = z.object({
  environment: z.enum(["development", "production", "staging"]),
  privacyPolicyUrl: z.string().url(),
  termsOfServiceUrl: z.string().url(),
  firebaseApiKey: z.string().min(1),
  firebaseAuthDomain: z.string().min(1),
  firebaseProjectId: z.string().min(1),
  firebaseStorageBucket: z.string().min(1),
  firebaseMessagingSenderId: z.string().min(1),
  firebaseAppId: z.string().min(1),
  firebaseMeasurementId: z.string().min(1),
  firebaseDatabaseURL: z.string().min(1),
  DISCORD_WEBHOOK_URL: z.string().url().optional(),
  TESTING_DATABASE_URL: z.string().url().optional()
})
// write as complex logic and checking you want to do in refine
.refine((env) => {
		if (env.ENVIRONMENT === "prod" && env.DB_TEST_URL) {
			return {
				path: "DB_TEST_URL",
				message: "DB_TEST_URL is not allowed in production",
			};
		}
		return true;
	});

// conditionally handle env enviornment wise

const envDevFields = z.object({
	ENVIRONMENT: z.literal("dev"),
});

// ex. discord webhook url is not optional in other envionments
const envProdFields = z.object({
	ENVIRONMENT: z.literal("prod"),
	DISCORD_WEBHOOK_URL: z.string().url(),
});

const envStagingFields = z.object({
	ENVIRONMENT: z.literal("staging"),
	DISCORD_WEBHOOK_URL: z.string().url(),
});

const conditionalEnvSchema = z.discriminatedUnion("ENVIRONMENT", [
	envDevFields,
	envProdFields,
	envStagingFields,
]);

// get types and parsed env:
export const envSchema = z.intersection(envDefaultFields, conditionalEnvSchema);
export const env = envSchema.parse(process.env);
export type Env = z.infer<typeof envSchema>;

export const isDev = env.environment === "development";
export const isProd = env.environment === "production";

```

benefit of using above method to handle env are:
- one central place where one can figure out what is happening related to env.
- type completion, safety and no more confusion that environment is `dev` or `development` no it must be `prod` or `production`.
- conditionally you can write as complex logic and parse things as your wish.
- App will not start until proper env is not present, which shouldn't in my opinion.

>[!note]
> In react native if you are using [[What is Expo & Why is Expo? | Expo]], your variables need to start with [EXPO_PUBLIC](https://docs.expo.dev/guides/environment-variables/), and you don't need any other lib to install to handle env for you it will handle it for you, if you are using plain react-native, use [react-native-dotenv](https://www.npmjs.com/package/react-native-dotenv) and for other  [dotenv](https://www.npmjs.com/package/dotenv) is the OG.

Oh you also want to use different name in env but different name in the code?

```typescript
// use parse method to map your env to desired env variable you want
export const env = envSchema.parse({
	companyName: process.env.company_name
	...
});

```

shoutout to [Krishna](https://x.com/krishnaa404) for introducing me to this method.

I am still updating my notes so bear with me, for updates follow me on [twitter](https://x.com/KaranJanthe).