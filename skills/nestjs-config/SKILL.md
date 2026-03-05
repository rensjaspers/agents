---
name: nestjs-config
description: NestJS configuration guidelines covering ConfigModule setup, explicit imports, environment variable validation, and the correct fallback pattern. Use when setting up or modifying configuration in a NestJS application.
---

# NestJS Config

---

## ConfigModule Setup

Always use `@nestjs/config`. **Never** set `isGlobal: true` — every module that needs configuration must import `ConfigModule` explicitly. This keeps dependencies visible and modules self-describing.

```ts
// app.module.ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      validate, // see Validation section below
    }),
  ],
})
export class AppModule {}
```

```ts
// cats/cats.module.ts  ← explicit import required
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [ConfigModule],
  providers: [CatsService],
})
export class CatsModule {}
```

---

## Validation

Validate **all** environment variables at startup. Follow the [NestJS config validation docs](https://docs.nestjs.com/techniques/configuration#schema-validation). Use either Joi or class-validator/class-transformer:

**Joi (inline):**

```ts
import * as Joi from 'joi';

ConfigModule.forRoot({
  validationSchema: Joi.object({
    DATABASE_URL: Joi.string().required(),
    PORT: Joi.number().default(3000),
  }),
});
```

**class-validator (custom validate function):**

```ts
import { plainToInstance } from 'class-transformer';
import { IsNumber, IsString, validateSync } from 'class-validator';

class EnvironmentVariables {
  @IsString()
  DATABASE_URL: string;

  @IsNumber()
  PORT: number;
}

function validate(config: Record<string, unknown>) {
  const validatedConfig = plainToInstance(EnvironmentVariables, config, {
    enableImplicitConversion: true,
  });
  const errors = validateSync(validatedConfig);
  if (errors.length > 0) {
    throw new Error(errors.toString());
  }
  return validatedConfig;
}
```

---

## Fallback Values

Use the **second argument** of `ConfigService.get()` for fallback values — do **not** use the `??` or `||` operators as a fallback.

```ts
// ✅ Correct
const port = this.configService.get<number>('PORT', 3000);

// ❌ Avoid
const port = this.configService.get<number>('PORT') ?? 3000;
```
