ulw

AUTOMATIC LSP SYSTEM CHECK

Run these checks and output ✅/⚠️/❌ status only:

1. Servers: All language servers running (pyright, typescript-language-server, gopls, rust-analyzer)?
2. Config: opencode.json has LSP settings and agents have permission?
3. Python: Test hover on a simple def statement - does LSP respond?
4. TypeScript: Test diagnostics on undefined variable - caught?
5. Performance: Response time < 500ms for hover?
6. Errors: Any LSP-related errors in logs or output?

OUTPUT ONLY:
✅/⚠️/❌ Servers
✅/⚠️/❌ Config  
✅/⚠️/❌ Python LSP
✅/⚠️/❌ TypeScript LSP
✅/⚠️/❌ Performance
✅/⚠️/❌ Errors

