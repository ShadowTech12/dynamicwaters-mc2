# MC2 fork: Fabric 1.20.1 port (paused)

Fork of [AdamNewGames/dynamicwaters-public](https://github.com/AdamNewGames/dynamicwaters-public), MPL-2.0.
Modified files stay under MPL-2.0; upstream licences and notices are unchanged.

Status (2026-11-09, branch `mc2-fabric-1.20.1`):
- Stonecutter target `1.20.1-fabric` added (`settings.gradle.kts`, `stonecutter.properties.toml`:
  fabric-api 0.92.7+1.20.1, modmenu 7.2.2, parchment 2023.09.03, Forge Config API Port 8.0.3).
- Dependencies resolve. `:1.20.1-fabric:compileJava` fails at javac's 100-error cap. Groups:
  - NeoForge `ModConfigSpec` in `Config`, `client/gui/*`: needs the `<1.21` string replacements from
    `build.forge.gradle.kts` copied into `build.fabric.gradle.kts` (FCAP 8 ships `ForgeConfigSpec`,
    `ModConfig`, `fuzs.forgeconfigapiport.api.config.v2.ForgeConfigRegistry`/`ModConfigEvents`).
  - `platform/fabric/FabricEntrypoint`, `FabricClientEntrypoint`, `platform/Platform`: 1.21 payload
    networking (`PayloadTypeRegistry`, `TYPE`, `STREAM_CODEC`) -> 1.20.1 `ResourceLocation` + `FriendlyByteBuf`
    channels; FCAP NeoForge v4 registry -> v2 `ForgeConfigRegistry`; `connection` is private.
  - Smaller: `ModBlocks`, `ModTags`, `ModSounds`, `Registrar`, `AbridgedCompat`, `FlowWandItem`,
    `TitleScreenMixin`, `ConfigurationScreenMixin`, `mixin/fabric/PlayerChunkSenderFabricMixin`
    (`PlayerChunkSender` is 1.20.2+).
- Build: `JAVA_HOME=/usr/lib/jvm/java-25-openjdk sh ./gradlew :1.20.1-fabric:compileJava`
  (`gradlew` lost its exec bit in the clone).

The pack uses Streams Reflowing instead for now.
