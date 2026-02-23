---
name: nestjs-logging
description: NestJS logging guidelines with a dedicated LogModule and LogService that provide ISO timestamps and respect configured log levels.
---

# NestJS Logging

---

## Pattern

Use a dedicated `LogModule` + `LogService` based on NestJS `ConsoleLogger`.

- Do not add Pino, Winston, or other third-party loggers unless explicitly requested.
- Register logging through DI, not through ad-hoc logger instances.

---

## LogService (ISO timestamps + loglevel)

Create a `LogService` that extends `ConsoleLogger`, overrides `getTimestamp()` to return ISO 8601, and applies log levels from `LOG_LEVEL`.

```ts
import { ConsoleLogger, Injectable, LogLevel } from "@nestjs/common";
import { ConfigService } from "@nestjs/config";

@Injectable()
export class LogService extends ConsoleLogger {
  private static readonly logLevelMap: Record<string, LogLevel[]> = {
    error: ["error"],
    warn: ["error", "warn"],
    info: ["error", "warn", "log"],
    debug: ["error", "warn", "log", "debug", "verbose"],
  };

  constructor(
    private readonly configService: ConfigService<
      Record<string, unknown>,
      false
    >
  ) {
    super(LogService.name, {
      timestamp: true,
    });
    this.setLogLevels(this.resolveLogLevels());
  }

  protected getTimestamp(): string {
    return new Date().toISOString();
  }

  private resolveLogLevels(): LogLevel[] {
    const rawLevel = this.configService.get<string>("LOG_LEVEL", "info");
    const normalizedLevel = rawLevel.toLowerCase();
    return (
      LogService.logLevelMap[normalizedLevel] ?? LogService.logLevelMap.info
    );
  }
}
```

---

## LogModule

Provide and export `LogService` from a separate module:

```ts
import { Module } from "@nestjs/common";
import { ConfigModule } from "@nestjs/config";
import { LogService } from "./log.service";

@Module({
  imports: [ConfigModule],
  providers: [LogService],
  exports: [LogService],
})
export class LogModule {}
```

Load `LogModule` in `AppModule` imports.

---

## Bootstrap in `main.ts`

Use Nest buffering and switch to the DI logger globally:

```ts
const app = await NestFactory.create(AppModule, {
  bufferLogs: true,
});
app.useLogger(app.get(LogService));
```

---

## Usage

Inject `LogService` into services/controllers and do not use `console.log`.

```ts
@Injectable()
export class CatsService {
  constructor(private readonly logger: LogService) {}

  findAll() {
    this.logger.log("Finding all cats");
  }
}
```
