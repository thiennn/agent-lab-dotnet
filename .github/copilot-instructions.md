# Copilot Workspace Instructions

**Soc Ops** — Social Bingo game, Blazor WebAssembly (.NET 10). Players mark 5×5 board squares when they find matching people.

## Commands

```bash
dotnet build SocOps/SocOps.csproj          # Build (must pass before committing)
dotnet run --project SocOps/SocOps.csproj  # Dev server → http://localhost:5166
dotnet test                                 # Run tests
```

## Architecture

```
SocOps/
├── Components/   # StartScreen, GameScreen, BingoBoard, BingoSquare, BingoModal
├── Models/       # GameState (Start→Playing→Bingo), BingoSquareData, BingoLine
├── Services/     # BingoGameService (state + localStorage), BingoLogicService (pure)
├── Data/         # Questions.cs — bingo prompts pool
├── Pages/        # Home.razor — single routable page
└── wwwroot/css/app.css  # Custom utility CSS (Tailwind-like)
```

## Key Patterns

- `BingoGameService` owns state; components subscribe to `OnStateChanged` → call `StateHasChanged()`
- `BingoLogicService` is **stateless** — pass board in, get board out
- localStorage persistence via `IJSRuntime`; fire-and-forget: `_ = SomeAsync()`
- Use `EventCallback<T>` not `Action<T>` for component events
- New services → `Scoped` in `Program.cs`

## Styling

Custom utilities in `wwwroot/css/app.css` — use existing classes first, add new themes via CSS variables.
Key classes: `.flex`, `.grid`, `.grid-cols-5`, `.bg-accent`, `.bg-marked`, `.p-1`–`.p-6`, `.max-w-md`

## C# Conventions

`Nullable` + `ImplicitUsings` enabled. PascalCase public members. Avoid `!` null suppression.

## Agents

| Agent | Purpose |
|---|---|
| `Pixel Jam` | Iterative UI design with browser preview |
| `Quiz Master` | Generate themed icebreaker questions |
| `TDD Supervisor` | Orchestrate Red→Green→Refactor cycle |
| `ui-review` | Review UI components |

## Workshop

Start at [workshop/00-overview.md](workshop/00-overview.md). Solutions in `.solutions/step-*/`.
