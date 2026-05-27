# Backend (dotnet)

Purpose
- Brief description: .NET backend services for the monorepo.

Local run (replace <project-path> with actual project file or solution):

```bash
dotnet restore
dotnet build
dotnet test
# run (example)
dotnet run --project <project-path>
```

Notes
- Put service projects under this folder, keep shared libraries in `../../packages/shared`.
- Follow testing requirements in `../../CLAUDE.md`.
