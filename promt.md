cd frontend && grep -rn "Font.register" src/ && echo "---FONTS---" && grep -rn "fontFamily" src/components/pdf/ | head -30
