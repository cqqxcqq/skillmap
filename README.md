# Skills Map

A browser-based skill tracking and visualization tool. Map your technical abilities, track progress over time, and discover connections between skills using an interactive network graph.

Built as a single HTML file with no build process required. Data persists locally using IndexedDB with localStorage fallback.

---

## Features

### Skill Management
- Numeric proficiency tracking (0-100% scale)
- Hours invested per skill
- Sub-skills with parent-child relationships
- Evidence/proof URLs for portfolio links
- Skill decay detection (marks skills as "rusty" after 6 months of inactivity)

### Visualization
- Interactive network graph powered by vis.js
- Node size scales with proficiency level
- Color coding by progress tier or category
- Timeline slider to view skill acquisition over time
- Focus mode to highlight a skill and its direct connections
- Manual connection drawing between nodes

### AI Integration (Optional)
- Natural language skill input parsing
- Automatic relationship detection between skills
- Job title matching based on skill profile
- Learning roadmap generation
- Skill quiz generation with progress rewards
- Automatic skill categorization (Frontend, Backend, DevOps, etc.)

Requires a free Groq API key. The application works without AI features using basic text parsing.

### Data Portability
- JSON export/import
- Shareable read-only URLs (Base64 encoded)
- PNG image export of the graph
- Markdown resume generation grouped by proficiency level
- Automatic localStorage backup

### Interface
- Two visual themes: Scrapbook (light) and Blueprint (dark)
- Undo/redo support (Ctrl+Z / Ctrl+Y)
- Right-click context menu on skills
- Mobile responsive layout

---

## Installation

No installation required. Download the HTML file and open it in a browser.

```bash
# Clone the repository
git clone https://github.com/yourusername/skills-map.git

# Open in browser
open skills-map.html
```

Or serve it locally:

```bash
python -m http.server 8000
# Navigate to http://localhost:8000/skills-map.html
```

---

## Usage

### Adding Skills

Type naturally in the chat input:

```
javascript 80%
python intermediate
react, nodejs, sql
learning rust
```

Or click the + button to open the manual entry form with full control over all fields.

### Skill Data Structure

Each skill contains:

| Field | Type | Description |
|-------|------|-------------|
| id | string | Normalized identifier |
| name | string | Display name |
| progress | number | 0-100 proficiency percentage |
| hours | number | Time invested |
| description | string | Brief description |
| proof | string | URL to project/portfolio |
| parentId | string/null | Parent skill ID for sub-skills |
| lastUsed | timestamp | Last practice date |
| dateAdded | timestamp | Creation date |
| category | string/null | Auto-assigned category |

### Connections

Connections can be created three ways:

1. **AI-generated**: Type "connect my skills" in chat
2. **Manual drawing**: Enable draw mode and drag between nodes
3. **Import**: Include in JSON import

Connection types:
- `prerequisite` - Skill A is required for Skill B
- `related` - Skills are commonly used together
- `manual` - User-defined connection

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| Ctrl+Z | Undo |
| Ctrl+Y | Redo |
| Right-click | Context menu |
| Enter | Send chat message |

### Context Menu Actions

Right-click on any skill node or sticker:

- Edit Skill - Open edit modal
- Refresh - Update lastUsed timestamp
- Add Sub-Skill - Create child skill
- Test Me - Generate quiz question (requires API)
- Freeze Position - Lock node placement
- Delete - Remove skill and connections

---

## AI Setup

The AI features are optional and require a Groq API key.

1. Get a free API key at https://console.groq.com
2. Click the key button in the header
3. Paste your API key

The key is stored in localStorage and never sent to any server other than Groq's API.

### AI Commands

| Command | Description |
|---------|-------------|
| connect my skills | Analyze and create logical connections |
| roast my stack | Humorous critique of skill choices |
| job match | Career title matching with percentages |
| categorize | Assign categories to all skills |
| roadmap to [skill] | Learning path from current skills |

---

## Data Format

### Export Format

```json
{
  "skills": [
    {
      "id": "javascript",
      "name": "JavaScript",
      "progress": 80,
      "hours": 500,
      "description": "Web programming",
      "proof": "https://github.com/user/project",
      "parentId": null,
      "lastUsed": 1699900000000,
      "dateAdded": 1699800000000,
      "category": "Frontend"
    }
  ],
  "relations": [
    {
      "from": "javascript",
      "to": "react",
      "type": "prerequisite",
      "reason": "React is a JavaScript library"
    }
  ],
  "exportedAt": "2024-01-15T10:30:00.000Z"
}
```

### Shareable URLs

The share feature encodes skill data as Base64 in the URL hash:

```
https://example.com/skills-map.html#data=eyJza2lsbHMiOlt7...
```

Recipients can view the data in read-only mode.

---

## Technical Details

### Dependencies

- [vis.js](https://visjs.org/) - Network graph visualization (loaded via CDN)
- [Google Fonts](https://fonts.google.com/) - Caveat, Inter, JetBrains Mono

### Browser Storage

- **IndexedDB**: Primary storage for skills and relations
- **localStorage**: API key, theme preference, backup data

### Browser Support

Tested on:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

Requires JavaScript enabled and IndexedDB support.

---

## File Structure

Single HTML file containing:

```
skills-map.html
├── <style>        - All CSS including theme variables
├── <body>         - Modal templates and main layout
└── <script>       - Application logic
    ├── State management
    ├── IndexedDB operations
    ├── Chat/input processing
    ├── AI integration (Groq)
    ├── Skill CRUD operations
    ├── Network rendering
    ├── Import/export functions
    └── Undo/redo history
```

---

## Customization

### Adding Themes

Themes are defined using CSS custom properties. Add a new theme by creating a data attribute selector:

```css
[data-theme="custom"] {
    --bg-primary: #1a1a1a;
    --bg-secondary: #2d2d2d;
    --border-color: #444;
    --text-primary: #eee;
    /* ... other variables */
}
```

### Modifying Categories

Category colors are defined in the `getCategoryColor` function:

```javascript
const colors = {
    'Frontend': '#61dafb',
    'Backend': '#68d391',
    'DevOps': '#fc8181',
    // Add custom categories here
};
```

---

## Known Limitations

- Large graphs (100+ nodes) may experience performance degradation
- Shared URLs have length limits in some browsers (approximately 2000 characters)
- Image export captures current viewport only, not full graph
- Sub-skills are not included in network graph, only in sticker view
- AI features require internet connection and valid API key

---

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make changes to the HTML file
4. Test in multiple browsers
5. Submit a pull request

Keep changes within the single-file architecture. External dependencies should be loaded via CDN.

---

## License

MIT License

Copyright (c) 2024

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.