# DesignCraft — funcionalidades e módulos

Última atualização: 2026-10-28.

## Fluxo principal

| Etapa | Entradas | Responsabilidade e dependências | Saídas |
|---|---|---|---|
| Abrir e editar | Documentos, histórias de texto e imagens vinculadas | [doc](../../crates/doc/), [images](../../crates/images/), [textimport](../../crates/textimport/), [idml](../../crates/idml/), [engine](../../crates/engine/) | Document com páginas, spreads e histórias encadeadas |
| Avaliar e produzir saída | Estado editado e parâmetros de saída | [compose](../../crates/compose/), [render](../../crates/render/), [pdf](../../crates/pdf/), [epub](../../crates/epub/), [format](../../crates/format/) | Páginas compostas, PDF, EPUB e arquivo nativo |
| Acesso pelo usuário ou agente | Gestos, argumentos CLI ou chamadas estruturadas | [apps](../../apps/), [engine](../../crates/engine/) e interfaces descritas abaixo | Estado atualizado, resposta ou erro; comportamento detalhado nos contratos |

A tabela mostra o caminho conceitual, não uma garantia de suporte a todos os formatos. APIs, estruturas, unidades, efeitos colaterais e limites ficam na [arquitetura](../architecture.md), nos documentos de assunto e nos módulos indicados.

## Contratos e exemplos de uso

- [typography-parity.md](../typography-parity.md)
- [agents.md](../agents.md)
- [pdf-backend.md](../pdf-backend.md)
- [graphic-layer-options.md](../graphic-layer-options.md)
- [control-protocol.md](../control-protocol.md)
- [mcp.md](../mcp.md)

## Inventário de bibliotecas

Responsabilidades resumidas a partir dos manifestos e comentários de módulo do checkout. As dependências listadas são as internas diretas declaradas em `dependencies` (incluindo condicionais); não incluem dependências de teste/build nem expansão de features. O manifesto de cada linha é a fonte exata.

| Módulo / manifesto | Responsabilidade declarada | Dependências internas diretas |
|---|---|---|
| [color](../../crates/color/Cargo.toml) | DesignCraft color | Nenhuma declarada nesta categoria |
| [compose](../../crates/compose/Cargo.toml) | DesignCraft text composition: paragraph composer (Knuth–Plass), hyphenation, justification, columns, threading, wrap | `designcraft-color`, `designcraft-doc`, `designcraft-fonts`, `designcraft-geom` |
| [doc](../../crates/doc/Cargo.toml) | DesignCraft document model: spreads, pages, parents, layers, frames, stories, styles, swatches | `designcraft-color`, `designcraft-geom` |
| [engine](../../crates/engine/Cargo.toml) | DesignCraft engine: session, documents, history, selection, command registry, tool hosting | `designcraft-color`, `designcraft-compose`, `designcraft-doc`, `designcraft-epub`, `designcraft-fonts`, `designcraft-format`, `designcraft-geom`, `designcraft-idml`, `designcraft-images`, `designcraft-pdf`, `designcraft-pptx`, `designcraft-render`, `designcraft-textimport`, `designcraft-tools` |
| [epub](../../crates/epub/Cargo.toml) | DesignCraft EPUB 3 (reflowable) export: stories in reading order, CSS from styles, images | `designcraft-color`, `designcraft-doc` |
| [fonts](../../crates/fonts/Cargo.toml) | DesignCraft fonts: font database (bundled OFL + system), metrics, glyph outlines, shaping | Nenhuma declarada nesta categoria |
| [format](../../crates/format/Cargo.toml) | DesignCraft native file format: zip with document.json and embedded assets; versioned | `designcraft-doc`, `designcraft-images` |
| [geom](../../crates/geom/Cargo.toml) | DesignCraft geometry: anchor paths, shapes, units and measurements, hit testing | Nenhuma declarada nesta categoria |
| [idml](../../crates/idml/Cargo.toml) | DesignCraft IDML (InDesign Markup Language) import and export | `designcraft-color`, `designcraft-doc`, `designcraft-geom`, `designcraft-images` |
| [images](../../crates/images/Cargo.toml) | DesignCraft placed-graphic decoding: raster formats (PNG, JPEG, GIF, WebP, TIFF, BMP, PSD) and SVG | `designcraft-color`, `designcraft-fonts` |
| [mcp](../../crates/mcp/Cargo.toml) | DesignCraft MCP server: JSON-RPC 2.0 over stdio, driving a running app or a headless session | `designcraft-engine`, `designcraft-geom`, `designcraft-render`, `designcraft-tools` |
| [pdf](../../crates/pdf/Cargo.toml) | DesignCraft PDF export (krilla): real text with embedded font subsets, CMYK/spot colour, bleed and printer's marks | `designcraft-color`, `designcraft-compose`, `designcraft-doc`, `designcraft-fonts`, `designcraft-geom`, `designcraft-images` |
| [pptx](../../crates/pptx/Cargo.toml) | DesignCraft PowerPoint (PPTX) export: pages as slides of editable shapes, pictures and text | `designcraft-color`, `designcraft-compose`, `designcraft-doc`, `designcraft-fonts`, `designcraft-geom`, `designcraft-images` |
| [render](../../crates/render/Cargo.toml) | DesignCraft renderer: spreads, pages, frames, composed text and images to pixels (vello_cpu) | `designcraft-color`, `designcraft-compose`, `designcraft-doc`, `designcraft-fonts`, `designcraft-geom`, `designcraft-images` |
| [textimport](../../crates/textimport/Cargo.toml) | DesignCraft text import: plain text, Word (.docx) and RTF into stories | `designcraft-doc` |
| [tools](../../crates/tools/Cargo.toml) | DesignCraft tools: pointer events in, commands and overlays out (Selection, Direct Selection, frames, Type, Hand, Zoom…) | `designcraft-color`, `designcraft-compose`, `designcraft-doc`, `designcraft-geom` |
| [ui-egui](../../crates/ui-egui/Cargo.toml) | DesignCraft egui frontend (InDesign-style UI); swappable — nothing below depends on it | `designcraft-color`, `designcraft-compose`, `designcraft-doc`, `designcraft-engine`, `designcraft-fonts`, `designcraft-geom`, `designcraft-render`, `designcraft-tools` |

Ao mudar uma entrada, saída ou dependência, atualize primeiro o contrato do módulo e depois esta navegação. [Voltar ao índice](../INDEX.md).
