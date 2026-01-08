# GRIPS Methodology Releases

This directory contains snapshots of released versions of the GRIPS methodology.

## Version History

### v0.0.1 (2026-01-08)
- **Git Tag**: `v0.0.1`
- **Commit**: `b9996f7`
- **Status**: First public release shared with design partners
- **Contents**: `0.0.1/methodology.md`

**Major Features:**
- Core GRIPS workflow (Requirements → Design → Implementation Plan → Code)
- Layer separation and concerns
- Q&A session process
- Review cycles with Critic Markup
- Change propagation (upward, downward, bidirectional)
- Commit strategy
- Implementation plan structure with phases and steps
- **NEW**: Comprehensive guidance on breaking down specs into implementation plans (Section 4.2)
- **NEW**: Enhanced approval gate review process (Section 5.1 Stage 4)
- Status tracking with PROJECT-STATUS.md
- Resumability instructions for agents

**Known Limitations:**
- Agent project management and phase guidance section not yet complete (planned for v0.0.2)

## Using Released Versions

### To reference a specific version:
```bash
# View a release via filesystem
cat releases/0.0.1/methodology.md

# Or checkout the git tag
git checkout v0.0.1
```

### To reference in project METHODOLOGY.md files:
```markdown
This project uses the GRIPS methodology at version 0.0.1.

For more information, visit: https://github.com/yourorg/grips

Reference: releases/0.0.1/methodology.md
Git tag: v0.0.1
```

## Version Numbering

GRIPS uses semantic-style versioning:
- **0.x.x**: Prototype/development versions
- **1.x.x**: Stable release (MVP)
- **2.x.x+**: Major expansions

### Increment rules:
- **Major** (x.0.0): Breaking changes to methodology structure
- **Minor** (0.x.0): New sections, significant additions
- **Patch** (0.0.x): Clarifications, minor fixes, examples
