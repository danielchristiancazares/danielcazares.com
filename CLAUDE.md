# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static personal portfolio website for Daniel Cazares. Single-page site with no build system, no JavaScript, and no external dependencies beyond Google Fonts.

## Structure

- `index.html` - Entire site in one file (HTML + embedded CSS)

## Development

Open `index.html` directly in a browser. No server or build step required.

## Design System

CSS custom properties defined in `:root`:
- `--paper`, `--ink`, `--muted` - Core colors
- `--accent` (orange), `--accent-2` (teal) - Accent colors
- `--panel`, `--shadow`, `--radius` - Card/component styling

Typography uses Fraunces (headings) and Space Grotesk (body) via Google Fonts.

## Responsive Breakpoints

- 900px: Hero switches from 2-column to single-column
- 600px: Tighter padding, stacked layouts for writing items and footer
