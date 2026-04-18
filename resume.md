# Resume Management

Resume and cover letter management tool with generation and preview.

### config
```ini
[resume]
  dir=${HOME}/Projects/gbraad-pages/resume
  port=2081
```

### checkout
```sh
if [ -d "$RESUME_DIR" ]; then
    echo "Resume directory already exists: $RESUME_DIR"
    exit 1
fi

echo "Setting up resume directory structure..."
echo ""

# Create main directory
mkdir -p "$RESUME_DIR"

# Create subdirectories
mkdir -p "$RESUME_DIR/public"
mkdir -p "$RESUME_DIR/assets"

# Create README
cat > "$RESUME_DIR/README.md" <<'EOF'
# Resume

Professional resume and cover letter management.

## Structure

- `public/` - Generated files (HTML, PDF, DOCX)
- `assets/` - Templates and generation assets
- `resume.md` - Main resume
- `resume-updated.md` - Updated resume
- `cover-letter*.md` - Cover letters
- `recommendations.md` - Recommendations

## Usage

Use the `resume` actionfile:

```bash
app resume stats        # Show statistics
app resume edit         # Edit resume/cover letter
app resume generate     # Generate files
app resume preview      # Preview in browser
app resume build-serve  # Build and serve
```

## Generation

Add generation scripts:
- `generate.sh` - Standard format
- `generate-modern.sh` - Modern theme
- `generate-vibrant.sh` - Vibrant theme
EOF

# Create basic resume template
cat > "$RESUME_DIR/resume.md" <<'EOF'
# Your Name

Email: your.email@example.com | Phone: +1-XXX-XXX-XXXX | Location: City, Country

## Summary

[Your professional summary]

## Experience

### Job Title
**Company Name** | Location | Start Date - End Date

- Achievement 1
- Achievement 2
- Achievement 3

## Education

### Degree
**University Name** | Location | Year

## Skills

- Skill 1
- Skill 2
- Skill 3
EOF

echo "- Created directory: $RESUME_DIR"
echo "- Created subdirectories: public/, assets/"
echo "- Created README.md"
echo "- Created resume.md template"
echo ""
echo "Resume directory is ready!"
echo ""
echo "Next steps:"
echo "  1. Edit resume.md with your information"
echo "  2. Add generation scripts to assets/"
echo "  3. Use 'app resume generate' to build files"
```

### info
```sh
echo "Resume Configuration"
echo "===================="
echo ""
echo "Resume Directory: $RESUME_DIR"

if [ -d "$RESUME_DIR" ]; then
    echo "- Directory exists"
    echo ""
    echo "Structure:"
    [ -d "$RESUME_DIR/public" ] && echo "public/" || echo "no public/"
    [ -d "$RESUME_DIR/assets" ] && echo "assets/"
    
    echo ""
    echo "Contents:"
    echo "  Resume files:  $(find "$RESUME_DIR" -maxdepth 1 -name "resume*.md" -o -name "index.md" | wc -l)"
    echo "  Cover letters: $(find "$RESUME_DIR" -maxdepth 1 -name "cover-letter*.md" | wc -l)"
    
    if [ -d "$RESUME_DIR/public" ]; then
        echo "  Generated PDFs:  $(find "$RESUME_DIR/public" -name "*.pdf" 2>/dev/null | wc -l)"
        echo "  Generated HTML:  $(find "$RESUME_DIR/public" -name "*.html" 2>/dev/null | wc -l)"
        echo "  Generated DOCX:  $(find "$RESUME_DIR/public" -name "*.docx" 2>/dev/null | wc -l)"
    fi
else
    echo "- Directory not found"
    echo ""
    echo "Run 'app resume checkout' to set up the directory structure"
fi
```

### stats
```sh
echo "Resume Statistics"
echo "================="
echo ""

# Count source files
resume_files=$(find "$RESUME_DIR" -maxdepth 1 -name "resume*.md" -o -name "index.md" 2>/dev/null | wc -l)
cover_letters=$(find "$RESUME_DIR" -maxdepth 1 -name "cover-letter*.md" 2>/dev/null | wc -l)
recommendations=$([ -f "$RESUME_DIR/recommendations.md" ] && echo "1" || echo "0")

echo "Source Files:"
echo "  Resumes:         $resume_files"
echo "  Cover Letters:   $cover_letters"
echo "  Recommendations: $recommendations"

if [ -d "$RESUME_DIR/public" ]; then
    pdfs=$(find "$RESUME_DIR/public" -name "*.pdf" 2>/dev/null | wc -l)
    htmls=$(find "$RESUME_DIR/public" -name "*.html" 2>/dev/null | wc -l)
    docx=$(find "$RESUME_DIR/public" -name "*.docx" 2>/dev/null | wc -l)
    
    echo ""
    echo "Generated Files:"
    echo "  PDFs:  $pdfs"
    echo "  HTML:  $htmls"
    echo "  DOCX:  $docx"
fi

echo ""

# Check for generation scripts
if [ -x "$RESUME_DIR/generate.sh" ]; then
    echo "- generate.sh available"
fi
if [ -x "$RESUME_DIR/generate-modern.sh" ]; then
    echo "- generate-modern.sh available"
fi
if [ -x "$RESUME_DIR/generate-vibrant.sh" ]; then
    echo "- generate-vibrant.sh available"
fi
```

### list
```sh
# Build list of resume and related files
files=()

# Add resume files
for file in "$RESUME_DIR"/resume*.md "$RESUME_DIR"/index.md; do
    [ -f "$file" ] || continue
    filename=$(basename "$file")
    title=$(grep -m 1 "^# " "$file" 2>/dev/null | sed 's/^# //' || echo "Resume")
    files+=("$filename - $title|$file")
done

# Add cover letters
for file in "$RESUME_DIR"/cover-letter*.md; do
    [ -f "$file" ] || continue
    filename=$(basename "$file")
    title=$(grep -m 1 "^# " "$file" 2>/dev/null | sed 's/^# //' || echo "Cover Letter")
    files+=("$filename - $title|$file")
done

# Add recommendations
if [ -f "$RESUME_DIR/recommendations.md" ]; then
    files+=("recommendations.md - Recommendations|$RESUME_DIR/recommendations.md")
fi

if [ ${#files[@]} -eq 0 ]; then
    echo "No resume files found"
    exit 0
fi

# Use fzf to select and preview
selected=$(printf '%s\n' "${files[@]}" | fzf \
    --delimiter='|' \
    --with-nth=1 \
    --preview='cat {2}' \
    --preview-window=right:60%:wrap \
    --header='Select file to view (ESC to cancel)')

if [ -n "$selected" ]; then
    filepath=$(echo "$selected" | cut -d'|' -f2)
    cat "$filepath"
fi
```

### edit
```sh
# Build list of resume and related files
files=()

# Add resume files
for file in "$RESUME_DIR"/resume*.md "$RESUME_DIR"/index.md; do
    [ -f "$file" ] || continue
    filename=$(basename "$file")
    title=$(grep -m 1 "^# " "$file" 2>/dev/null | sed 's/^# //' || echo "Resume")
    files+=("$filename - $title|$file")
done

# Add cover letters
for file in "$RESUME_DIR"/cover-letter*.md; do
    [ -f "$file" ] || continue
    filename=$(basename "$file")
    title=$(grep -m 1 "^# " "$file" 2>/dev/null | sed 's/^# //' || echo "Cover Letter")
    files+=("$filename - $title|$file")
done

# Add recommendations
if [ -f "$RESUME_DIR/recommendations.md" ]; then
    files+=("recommendations.md - Recommendations|$RESUME_DIR/recommendations.md")
fi

if [ ${#files[@]} -eq 0 ]; then
    echo "No resume files found"
    exit 0
fi

# Use fzf to select and edit
selected=$(printf '%s\n' "${files[@]}" | fzf \
    --delimiter='|' \
    --with-nth=1 \
    --preview='cat {2}' \
    --preview-window=right:60%:wrap \
    --header='Select file to edit (ESC to cancel)')

if [ -n "$selected" ]; then
    filepath=$(echo "$selected" | cut -d'|' -f2)
    ${EDITOR:-vim} "$filepath"
fi
```

### generate
```sh
cd "$RESUME_DIR"

# Select generation type
scripts=()
[ -x "./generate.sh" ] && scripts+=("Standard (generate.sh)")
[ -x "./generate-modern.sh" ] && scripts+=("Modern Theme (generate-modern.sh)")
[ -x "./generate-vibrant.sh" ] && scripts+=("Vibrant Theme (generate-vibrant.sh)")
[ -x "./generate-cover-letter.sh" ] && scripts+=("Cover Letter (generate-cover-letter.sh)")
scripts+=("All")

if [ ${#scripts[@]} -eq 0 ]; then
    echo "No generation scripts found"
    exit 1
fi

selected=$(printf '%s\n' "${scripts[@]}" | fzf --header="Select generation type")

case "$selected" in
    "Standard (generate.sh)")
        ./generate.sh
        ;;
    "Modern Theme (generate-modern.sh)")
        ./generate-modern.sh
        ;;
    "Vibrant Theme (generate-vibrant.sh)")
        ./generate-vibrant.sh
        ;;
    "Cover Letter (generate-cover-letter.sh)")
        ./generate-cover-letter.sh
        ;;
    "All")
        echo "Generating all formats..."
        [ -x "./generate.sh" ] && ./generate.sh
        [ -x "./generate-modern.sh" ] && ./generate-modern.sh
        [ -x "./generate-vibrant.sh" ] && ./generate-vibrant.sh
        echo "- All formats generated"
        ;;
    *)
        echo "Cancelled"
        exit 1
        ;;
esac
```

### preview
```sh
cd "$RESUME_DIR"

if [ ! -d "./public" ]; then
    echo "No public directory found. Run 'app resume generate' first."
    exit 1
fi

# Find all generated files
files=()
for file in ./public/*.{html,pdf}; do
    [ -f "$file" ] || continue
    filename=$(basename "$file")
    files+=("$filename|$file")
done

if [ ${#files[@]} -eq 0 ]; then
    echo "No generated files found in public/"
    echo "Run 'app resume generate' first"
    exit 0
fi

# Use fzf to select
selected=$(printf '%s\n' "${files[@]}" | fzf \
    --delimiter='|' \
    --with-nth=1 \
    --header='Select file to preview (ESC to cancel)')

if [ -n "$selected" ]; then
    filepath=$(echo "$selected" | cut -d'|' -f2)
    
    # Detect file type and open appropriately
    case "$filepath" in
        *.html)
            if command -v firefox &> /dev/null; then
                firefox "$filepath" &
            elif command -v google-chrome &> /dev/null; then
                google-chrome "$filepath" &
            elif command -v xdg-open &> /dev/null; then
                xdg-open "$filepath" &
            else
                echo "No browser found. File: $filepath"
            fi
            ;;
        *.pdf)
            if command -v evince &> /dev/null; then
                evince "$filepath" &
            elif command -v xdg-open &> /dev/null; then
                xdg-open "$filepath" &
            else
                echo "No PDF viewer found. File: $filepath"
            fi
            ;;
        *)
            echo "Opening: $filepath"
            xdg-open "$filepath" &
            ;;
    esac
fi
```

### test-ats
```sh
cd "$RESUME_DIR"

# Select test type
tests=()
[ -x "./test-ats.sh" ] && tests+=("Quick ATS Test (test-ats.sh)")
[ -x "./test-ats-verbose.sh" ] && tests+=("Verbose ATS Test (test-ats-verbose.sh)")

if [ ${#tests[@]} -eq 0 ]; then
    echo "No ATS test scripts found"
    exit 1
fi

selected=$(printf '%s\n' "${tests[@]}" | fzf --header="Select ATS test type")

case "$selected" in
    "Quick ATS Test (test-ats.sh)")
        ./test-ats.sh
        ;;
    "Verbose ATS Test (test-ats-verbose.sh)")
        ./test-ats-verbose.sh
        ;;
    *)
        echo "Cancelled"
        exit 1
        ;;
esac
```

### build
```sh
cd "$RESUME_DIR"

echo "Building all resume formats..."
echo ""

# Run all available generation scripts
success=0
total=0

if [ -x "./generate.sh" ]; then
    total=$((total + 1))
    echo "==> Standard format"
    ./generate.sh && success=$((success + 1))
fi

if [ -x "./generate-modern.sh" ]; then
    total=$((total + 1))
    echo "==> Modern theme"
    ./generate-modern.sh && success=$((success + 1))
fi

if [ -x "./generate-vibrant.sh" ]; then
    total=$((total + 1))
    echo "==> Vibrant theme"
    ./generate-vibrant.sh && success=$((success + 1))
fi

echo ""
echo "Build complete: $success/$total succeeded"

if [ $success -lt $total ]; then
    exit 1
fi
```

### serve
```sh
if [ ! -d "$RESUME_DIR/public" ]; then
    echo "Error: public directory not found"
    echo "Run 'app resume build' first"
    exit 1
fi

cd "$RESUME_DIR/public"

echo "Starting HTTP server in $RESUME_DIR/public"
echo "Server running at: http://localhost:$RESUME_PORT"
echo ""
echo "Available files:"
ls -1 *.html *.pdf 2>/dev/null | sed 's/^/  - /'
echo ""
echo "Open in browser:"
echo "  http://localhost:$RESUME_PORT/index.html"
echo "  http://localhost:$RESUME_PORT/resume.html"
echo "  http://localhost:$RESUME_PORT/resume-modern.html"
echo ""
echo "Press Ctrl+C to stop"
echo ""

python3 -m http.server "$RESUME_PORT"
```

### build-serve
```sh
echo "Building resume..."
app resume build

if [ $? -eq 0 ]; then
    echo ""
    echo "Build successful! Starting server..."
    echo ""
    app resume serve
else
    echo "Build failed"
    exit 1
fi
```

### browse
```sh
while true; do
    action=$(echo -e "List Files\nEdit\nGenerate\nPreview\nTest ATS\nBuild All\nServe\nBuild & Serve\nStats\nExit" | fzf --header="Resume Management")
    
    case "$action" in
        "List Files")
            app resume list
            ;;
        "Edit")
            app resume edit
            ;;
        "Generate")
            app resume generate
            echo ""
            read -p "Press Enter to continue..."
            ;;
        "Preview")
            app resume preview
            ;;
        "Test ATS")
            app resume test-ats
            echo ""
            read -p "Press Enter to continue..."
            ;;
        "Build All")
            app resume build
            echo ""
            read -p "Press Enter to continue..."
            ;;
        "Serve")
            app resume serve
            ;;
        "Build & Serve")
            app resume build-serve
            ;;
        "Stats")
            app resume stats
            echo ""
            read -p "Press Enter to continue..."
            ;;
        "Exit"|"")
            break
            ;;
    esac
done
```

### quick
```sh
echo "=== Resume Quick Start ==="
echo ""

# Show stats
app resume stats

echo ""
echo ""

# Ask what to do
action=$(echo -e "Edit resume\nGenerate all\nGenerate specific\nPreview\nBuild & serve\nTest ATS\nNothing, just checking" | fzf --header="What would you like to do?")

case "$action" in
    "Edit resume")
        app resume edit
        ;;
    "Generate all")
        app resume build
        ;;
    "Generate specific")
        app resume generate
        ;;
    "Preview")
        app resume preview
        ;;
    "Build & serve")
        app resume build-serve
        ;;
    "Test ATS")
        app resume test-ats
        ;;
esac
```

### default alias run
```sh
app resume quick
```
