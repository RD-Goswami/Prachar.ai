# Prachar.ai Architecture Diagram Generator
# Generates professional-tier diagrams from DOT source

.PHONY: all png svg pdf clean help

# Default target
all: png svg pdf
	@echo "✅ All diagrams generated successfully!"
	@echo "📁 Files created:"
	@ls -lh system-architecture.png system-architecture.svg system-architecture.pdf

# Generate high-resolution PNG (300 DPI)
png:
	@echo "🎨 Generating PNG diagram (300 DPI)..."
	dot -Tpng -Gdpi=300 system-architecture.dot -o system-architecture.png
	@echo "✅ PNG generated: system-architecture.png"

# Generate scalable SVG
svg:
	@echo "🎨 Generating SVG diagram (scalable)..."
	dot -Tsvg system-architecture.dot -o system-architecture.svg
	@echo "✅ SVG generated: system-architecture.svg"

# Generate print-quality PDF
pdf:
	@echo "🎨 Generating PDF diagram (print quality)..."
	dot -Tpdf system-architecture.dot -o system-architecture.pdf
	@echo "✅ PDF generated: system-architecture.pdf"

# Generate low-res PNG for quick preview
preview:
	@echo "🎨 Generating preview PNG (150 DPI)..."
	dot -Tpng -Gdpi=150 system-architecture.dot -o system-architecture-preview.png
	@echo "✅ Preview generated: system-architecture-preview.png"

# Clean generated files
clean:
	@echo "🧹 Cleaning generated diagrams..."
	rm -f system-architecture.png
	rm -f system-architecture.svg
	rm -f system-architecture.pdf
	rm -f system-architecture-preview.png
	@echo "✅ Cleaned!"

# Validate DOT syntax
validate:
	@echo "🔍 Validating DOT syntax..."
	dot -Tpng system-architecture.dot -o /dev/null
	@echo "✅ DOT file is valid!"

# Open generated PNG (macOS)
open-png: png
	@echo "📂 Opening PNG..."
	open system-architecture.png

# Open generated SVG (macOS)
open-svg: svg
	@echo "📂 Opening SVG..."
	open system-architecture.svg

# Open generated PDF (macOS)
open-pdf: pdf
	@echo "📂 Opening PDF..."
	open system-architecture.pdf

# Help
help:
	@echo "Prachar.ai Architecture Diagram Generator"
	@echo ""
	@echo "Usage:"
	@echo "  make all       - Generate PNG, SVG, and PDF"
	@echo "  make png       - Generate high-res PNG (300 DPI)"
	@echo "  make svg       - Generate scalable SVG"
	@echo "  make pdf       - Generate print-quality PDF"
	@echo "  make preview   - Generate low-res PNG for quick preview"
	@echo "  make validate  - Validate DOT syntax"
	@echo "  make clean     - Remove generated files"
	@echo "  make open-png  - Generate and open PNG (macOS)"
	@echo "  make open-svg  - Generate and open SVG (macOS)"
	@echo "  make open-pdf  - Generate and open PDF (macOS)"
	@echo "  make help      - Show this help message"
	@echo ""
	@echo "Requirements:"
	@echo "  - Graphviz must be installed"
	@echo "  - macOS: brew install graphviz"
	@echo "  - Ubuntu: sudo apt-get install graphviz"
	@echo "  - Windows: choco install graphviz"
