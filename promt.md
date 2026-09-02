Remove-Item -Recurse -Force node_modules\@next\swc-win32-x64-msvc -ErrorAction SilentlyContinue
Remove-Item -Recurse -Force "$env:LOCALAPPDATA\next-swc" -ErrorAction SilentlyContinue
npm config set strict-ssl false
npm install @next/swc-win32-x64-msvc --save-dev --force
