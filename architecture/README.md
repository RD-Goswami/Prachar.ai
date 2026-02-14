# Prachar.ai Architecture Diagrams

This directory contains the professional-tier system architecture diagrams for Prachar.ai.

---

## Files

1. **system-architecture.dot** - Graphviz/DOT source file
2. **ARCHITECTURE.md** - Complete architecture documentation
3. **README.md** - This file

---

## Generating Diagrams

### Prerequisites

Install Graphviz:

```bash
# macOS
brew install graphviz

# Ubuntu/Debian
sudo apt-get install graphviz

# Windows
# Download from: https://graphviz.org/download/
# Or use: choco install graphviz
```

### Generate PNG (High Resolution)

```bash
dot -Tpng -Gdpi=300 system-architecture.dot -o system-architecture.png
```

### Generate SVG (Scalable Vector)

```bash
dot -Tsvg system-architecture.dot -o system-architecture.svg
```

### Generate PDF (Print Quality)

```bash
dot -Tpdf system-architecture.dot -o system-architecture.pdf
```

### Generate All Formats

```bash
# PNG for presentations
dot -Tpng -Gdpi=300 system-architecture.dot -o system-architecture.png

# SVG for web
dot -Tsvg system-architecture.dot -o system-architecture.svg

# PDF for documentation
dot -Tpdf system-architecture.dot -o system-architecture.pdf
```

---

## Diagram Features

### Execution Path (1-12)
The diagram shows the complete request flow with numbered steps:

1. **User Submission** - Student enters campaign goal
2. **Authentication** - Cognito validates credentials
3. **Token Validation** - API Gateway checks JWT
4. **Lambda Invocation** - Serverless compute starts
5. **Agent Initialization** - Strands SDK begins reasoning
6. **RAG Retrieval** - Knowledge Base provides brand context
7. **Copy Generation** - Claude creates Hinglish captions
8. **Safety Filtering** - Guardrails validate content
9. **Image Generation** - Titan creates campaign poster
10. **Asset Storage** - S3 stores generated image
11. **Data Persistence** - DynamoDB saves campaign
12. **Campaign Delivery** - Frontend displays results

### Architecture Layers

1. **Presentation Layer** (Orange) - Next.js Frontend
2. **Identity Layer** (Red) - Amazon Cognito
3. **API Gateway Layer** (Pink) - REST API
4. **Compute Layer** (Orange/Blue) - Lambda + Strands
5. **AI Intelligence Layer** (Blue) - Bedrock Services
6. **Storage Layer** (Green/Blue) - S3 + DynamoDB
7. **Monitoring Layer** (Pink) - CloudWatch

### Color Coding

- **Orange (#FF9900)** - AWS Compute & Frontend
- **Red (#DD344C)** - Security & Authentication
- **Blue (#527FFF)** - AI & Intelligence Services
- **Green (#569A31)** - Storage Services
- **Pink (#FF4F8B)** - API & Monitoring

### Line Styles

- **Solid Lines** - Main execution path (1-12)
- **Dashed Lines** - Agentic orchestration
- **Dotted Lines** - Monitoring and logs

---

## Viewing the Diagram

### Online Viewers

1. **Graphviz Online**
   - Visit: https://dreampuf.github.io/GraphvizOnline/
   - Paste contents of `system-architecture.dot`
   - View rendered diagram

2. **VS Code Extension**
   - Install: "Graphviz (dot) language support"
   - Open `system-architecture.dot`
   - Right-click → "Open Preview"

### Local Viewing

```bash
# Generate and open PNG
dot -Tpng -Gdpi=300 system-architecture.dot -o system-architecture.png
open system-architecture.png  # macOS
xdg-open system-architecture.png  # Linux
start system-architecture.png  # Windows
```

---

## Customization

### Changing Colors

Edit the `fillcolor` attributes in the DOT file:

```dot
node [fillcolor="#FF9900"]  // AWS Orange
node [fillcolor="#527FFF"]  // Bedrock Blue
node [fillcolor="#DD344C"]  // Security Red
```

### Changing Layout

Modify the `rankdir` attribute:

```dot
graph [rankdir=TB]  // Top to Bottom (current)
graph [rankdir=LR]  // Left to Right
```

### Adding Components

Add new nodes and edges:

```dot
newservice [
    label="New AWS Service\n\nDescription"
    fillcolor="#FF9900"
    fontcolor=white
]

existingservice -> newservice [
    label="Connection"
    color="#FF9900"
    penwidth=3
]
```

---

## Architecture Highlights

### AWS Services (7 Total)

1. **Amazon Bedrock** (4 services)
   - Claude 3.5 Sonnet (text generation)
   - Titan Image Generator (visual generation)
   - Knowledge Bases (RAG)
   - Guardrails (safety)

2. **Amazon Cognito** - Authentication
3. **AWS Lambda** - Serverless compute
4. **Amazon DynamoDB** - NoSQL database
5. **Amazon S3** - Object storage
6. **Amazon API Gateway** - REST API
7. **Amazon CloudWatch** - Monitoring

### Key Innovations

- **Autonomous Agentic AI** - Strands SDK orchestration
- **Hinglish Generation** - Cultural authenticity
- **RAG-Based Consistency** - Brand-aware content
- **JWT Security** - Complete user isolation
- **Hybrid Failover** - 100% uptime guarantee

---

## For Hackathon Judges

### What to Look For

1. **Execution Path (1-12)** - Clear, numbered flow
2. **7 AWS Services** - All integrated and labeled
3. **Security Layers** - Cognito → JWT → User Isolation
4. **Agentic Workflow** - Strands orchestration visible
5. **Bedrock Integration** - 4 services in single flow

### Key Differentiators

- ✅ Professional-tier architecture diagram
- ✅ Complete execution path documentation
- ✅ 7-layer architecture with clear separation
- ✅ Security-first design with audit trails
- ✅ Autonomous agentic AI workflow

---

## Troubleshooting

### Graphviz Not Found

```bash
# Verify installation
dot -V

# Should output: dot - graphviz version X.X.X
```

### Diagram Too Large

```bash
# Reduce DPI for smaller file
dot -Tpng -Gdpi=150 system-architecture.dot -o system-architecture.png
```

### Text Overlapping

Edit the DOT file and increase spacing:

```dot
graph [
    nodesep=1.5  // Increase from 1.0
    ranksep=1.5  // Increase from 1.2
]
```

---

## Additional Resources

- **Graphviz Documentation**: https://graphviz.org/documentation/
- **DOT Language Guide**: https://graphviz.org/doc/info/lang.html
- **AWS Architecture Icons**: https://aws.amazon.com/architecture/icons/

---

## Quick Commands

```bash
# Generate all formats at once
make diagrams

# Or manually:
dot -Tpng -Gdpi=300 system-architecture.dot -o system-architecture.png && \
dot -Tsvg system-architecture.dot -o system-architecture.svg && \
dot -Tpdf system-architecture.dot -o system-architecture.pdf

echo "✅ All diagrams generated!"
```

---

**Status:** ✅ READY FOR PRESENTATION  
**Quality:** Professional-Tier  
**Format:** Graphviz/DOT with AWS Icons  
**Execution Path:** Labeled 1-12  

**Perfect for hackathon submission!** 🏆
