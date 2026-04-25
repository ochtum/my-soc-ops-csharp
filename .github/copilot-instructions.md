# Copilot Instructions

## Mandatory Dev Checklist

Before every commit, run and verify all pass:

```bash
dotnet format SocOps/SocOps.csproj --verify-no-changes   # Lint
dotnet build SocOps/SocOps.csproj                        # Build
dotnet test                                               # Test (when tests exist)
```

Also verify: PascalCase for C# public members, no unused `@using` directives in Razor files.

## Project

**SocOps** — Social Bingo, Blazor WebAssembly (.NET 10). Dev server: `dotnet run --project SocOps/SocOps.csproj` → http://localhost:5166

## Architecture

| Path | Purpose |
|------|---------|
| `Components/` | BingoBoard, BingoSquare, BingoModal, GameScreen, StartScreen |
| `Services/` | `BingoGameService` (state + localStorage), `BingoLogicService` (pure logic) |
| `Models/` | `BingoSquareData`, `GameState` (Start/Playing/Bingo), `BingoLine` |
| `Data/Questions.cs` | 24 random prompts drawn per game; edit here to add questions |
| `Pages/Home.razor` | Only page — injects `BingoGameService` and wires all events |

## Key Conventions

- **State**: `BingoGameService` (scoped DI) notifies via `event Action? OnStateChanged`; persist with `_ = SaveGameStateAsync()` (fire-and-forget)
- **Components**: leaf components use `[Parameter]` + `EventCallback` only — no service injection
- **Board**: always `List<BingoSquareData>` of 25 items; index 12 is the free space
- **Styling**: compose from utilities in [app.css](../SocOps/wwwroot/css/app.css) — see [css-utilities](.github/instructions/css-utilities.instructions.md). No Tailwind CDN, no inline styles.
- **Questions**: keep inclusive and low-stakes — see [quiz-master agent](.github/agents/quiz-master.agent.md)
