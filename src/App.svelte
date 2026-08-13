<script>
    import { onMount } from "svelte";
    const person = {
        id: "person",
        label: "Luis Mauel",
        eyebrow: "Creative developer",
        description:
            "I connect ideas across design, technology, and culture, then turn those connections into clear, useful work.",
        kind: "person",
    };

    const categories = [
        {
            id: "tools",
            label: "Tools",
            eyebrow: "Build with",
            description:
                "The frameworks, platforms, and languages I use to turn ideas into working products.",
            kind: "category",
            accent: "#647b60",
            symbol: "01",
            items: [
                {
                    id: "sveltekit",
                    label: "SvelteKit",
                    eyebrow: "Web framework",
                    description:
                        "Fast, focused web experiences built with SvelteKit.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/354416/svelte-icon.svg",
                    ],
                },
                {
                    id: "react",
                    label: "React",
                    eyebrow: "Interface library",
                    description:
                        "Component-driven interfaces for flexible, interactive products.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/354259/react.svg",
                    ],
                },
                {
                    id: "microsoft-fabric",
                    label: "Microsoft Fabric",
                    eyebrow: "Data platform",
                    description:
                        "An end-to-end analytics platform for bringing data, engineering, and insight together.",
                    kind: "item",
                    logos: ["/logos/microsoft-fabric-color.svg?v=2"],
                },
                {
                    id: "github",
                    label: "GitHub",
                    eyebrow: "Code collaboration",
                    description:
                        "Versioned work, open collaboration, and reliable project delivery.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/353780/github-icon.svg",
                    ],
                },
                {
                    id: "visual-studio-code",
                    label: "Visual Studio Code",
                    eyebrow: "Development environment",
                    description:
                        "My focused workspace for building, testing, and refining code.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/354522/visual-studio-code.svg",
                    ],
                },
                {
                    id: "html-css",
                    label: "HTML / CSS",
                    eyebrow: "Visual structure",
                    description:
                        "Structure, typography, and visual systems that hold an experience together.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/452228/html-5.svg",
                        "https://www.svgrepo.com/show/452185/css-3.svg",
                    ],
                },
                {
                    id: "python",
                    label: "Python",
                    eyebrow: "Patterns and automation",
                    description:
                        "A practical language for exploring data, patterns, and useful automation.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/452091/python.svg",
                    ],
                },
                {
                    id: "java",
                    label: "Java",
                    eyebrow: "Application development",
                    description:
                        "Structured, portable software for dependable applications and services.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/452234/java.svg",
                    ],
                },
                {
                    id: "sql",
                    label: "SQL",
                    eyebrow: "Finding the signal",
                    description:
                        "A way to ask precise questions of messy information and make the answer useful.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/331760/sql-database-generic.svg",
                    ],
                },
            ],
        },
        {
            id: "ai",
            label: "AI",
            eyebrow: "Think alongside",
            description:
                "Tools I use to explore possibilities, sharpen questions, and move from thought to prototype.",
            kind: "category",
            accent: "#b66a4f",
            symbol: "02",
            items: [
                {
                    id: "codex",
                    label: "Codex",
                    eyebrow: "Build",
                    description:
                        "An engineering agent for turning scoped ideas into tested, working code.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/306500/openai.svg",
                    ],
                },
                {
                    id: "claude",
                    label: "Claude",
                    eyebrow: "Reason",
                    description:
                        "A thoughtful collaborator for close reading, reasoning, and clear writing.",
                    kind: "item",
                    logos: ["https://cdn.simpleicons.org/claude"],
                },
                {
                    id: "github-copilot",
                    label: "GitHub Copilot",
                    eyebrow: "Code alongside",
                    description:
                        "AI assistance embedded directly into the development workflow.",
                    kind: "item",
                    logos: ["https://cdn.simpleicons.org/githubcopilot"],
                },
                {
                    id: "openrouter",
                    label: "OpenRouter",
                    eyebrow: "Connect models",
                    description:
                        "One flexible interface for comparing and working across AI models.",
                    kind: "item",
                    logos: ["/logos/openrouter.svg"],
                },
                {
                    id: "machine-learning",
                    label: "Machine Learning",
                    eyebrow: "Find patterns",
                    description:
                        "Models and experiments that turn data into useful predictions and signals.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/421448/chip-intelligence-processor.svg",
                    ],
                },
                {
                    id: "pyspark",
                    label: "PySpark",
                    eyebrow: "Scale data",
                    description:
                        "Distributed Python workflows for transforming and analyzing large datasets.",
                    kind: "item",
                    logos: [
                        "https://www.svgrepo.com/show/341608/apache-spark.svg",
                    ],
                },
            ],
        },
        {
            id: "projects",
            label: "Projects",
            eyebrow: "Make the signal visible",
            description:
                "Experiments that connect information, interface, and a point of view.",
            kind: "category",
            accent: "#c7634a",
            symbol: "03",
            items: [],
        },
        {
            id: "companies",
            label: "Companies",
            eyebrow: "Build in context",
            description:
                "The kinds of places and collaborations where I look for the useful connection.",
            kind: "category",
            accent: "#8b7653",
            symbol: "04",
            items: [],
        },
    ];

    let view = "overview";
    let activeCategoryId = null;
    let selectedId = person.id;
    let returnFocusId = null;
    let viewportWidth = 1200;
    let viewportHeight = 800;
    let scrollProgress = 0;
    let isNavigating = false;
    let hoveredId = null;
    let transitionTimer;
    let renderedNodes = [];
    let renderedEdges = [];
    let layoutSignature = "";
    let previousEdges = [];
    let animationFrame = 0;

    onMount(() => {
        const updateViewport = () => {
            viewportWidth = window.innerWidth;
            viewportHeight = window.innerHeight;
        };

        const updateScrollProgress = () => {
            const graphSection = document.getElementById("graph");
            if (!graphSection) {
                return;
            }

            const entryDistance = Math.max(window.innerHeight * 0.72, 1);
            const progress =
                (entryDistance - graphSection.getBoundingClientRect().top) /
                entryDistance;
            scrollProgress = Math.min(1, Math.max(0, progress));
        };

        let scrollFrame = 0;
        const handleScroll = () => {
            if (scrollFrame) {
                return;
            }

            scrollFrame = requestAnimationFrame(() => {
                scrollFrame = 0;
                updateScrollProgress();
            });
        };

        updateViewport();
        updateScrollProgress();
        window.addEventListener("resize", updateViewport);
        window.addEventListener("scroll", handleScroll, { passive: true });

        return () => {
            window.removeEventListener("resize", updateViewport);
            window.removeEventListener("scroll", handleScroll);
            cancelAnimationFrame(scrollFrame);
            window.clearTimeout(transitionTimer);
            window.cancelAnimationFrame(animationFrame);
        };
    });

    $: isSmallViewport = viewportWidth <= 520;
    $: isMediumViewport = viewportWidth <= 900;

    function getGraphOrigin() {
        return { x: 50, y: 50 };
    }

    function getRadialRadii(scale = 1) {
        const width = Math.max(viewportWidth, 320);
        const height = Math.max(viewportHeight, 480);
        const widthRatio = isSmallViewport ? 0.31 : 0.245;
        const heightRatio = isSmallViewport ? 0.23 : 0.285;
        const radius =
            Math.min(width * widthRatio, height * heightRatio) * scale;

        return {
            radiusX: (radius / width) * 100,
            radiusY: (radius / height) * 100,
        };
    }

    function positionAroundCircle(
        index,
        total,
        radiusX,
        radiusY,
        startAngle = -Math.PI / 2,
        origin = { x: 50, y: 50 },
    ) {
        const angle = startAngle + (index * Math.PI * 2) / total;

        return {
            x: origin.x + Math.cos(angle) * radiusX,
            y: origin.y + Math.sin(angle) * radiusY,
        };
    }

    function getOverviewNodes() {
        const origin = getGraphOrigin();
        const { radiusX, radiusY } = getRadialRadii();

        return [
            { ...person, ...origin, delay: 0 },
            ...categories.map((category, index) => ({
                ...category,
                ...origin,
                ...positionAroundCircle(
                    index,
                    categories.length,
                    radiusX,
                    radiusY,
                    undefined,
                    origin,
                ),
                delay: index * 70,
            })),
        ];
    }

    function getCategoryNodes() {
        if (!activeCategory) {
            return [{ ...person, ...getGraphOrigin(), delay: 0 }];
        }

        const origin = getGraphOrigin();
        const { radiusX, radiusY } = getRadialRadii(0.9);

        return [
            {
                ...person,
                x: isSmallViewport ? 12 : 15,
                y: 50,
                breadcrumb: true,
                delay: 0,
            },
            {
                ...activeCategory,
                ...origin,
                open: true,
                delay: 0,
            },
            ...activeCategory.items.map((item, index) => ({
                ...item,
                parentId: activeCategory.id,
                ...origin,
                ...positionAroundCircle(
                    index,
                    activeCategory.items.length,
                    radiusX,
                    radiusY,
                    -Math.PI / 2 + Math.PI / 4,
                    origin,
                ),
                delay: 120 + index * 75,
            })),
        ];
    }

    function getNode(id) {
        return graphNodes.find((node) => node.id === id) ?? person;
    }

    function getRenderedNode(id) {
        return renderedNodes.find((node) => node.id === id) ?? getNode(id);
    }

    function easeInOut(progress) {
        return (
            progress *
            progress *
            progress *
            (progress * (progress * 6 - 15) + 10)
        );
    }

    function progressBetween(progress, start, end) {
        return Math.min(1, Math.max(0, (progress - start) / (end - start)));
    }

    function clamp(minimum, value, maximum) {
        return Math.min(maximum, Math.max(minimum, value));
    }

    function getNodeDiameter(node) {
        if (isSmallViewport) {
            if (node.kind === "person") {
                return node.breadcrumb ? 58 : 84;
            }

            if (node.kind === "item") {
                return 58;
            }

            return node.open ? 84 : 76;
        }

        if (node.kind === "person") {
            return node.breadcrumb
                ? clamp(68, viewportWidth * 0.06, 78)
                : clamp(96, viewportWidth * 0.085, 110);
        }

        if (node.kind === "item") {
            return clamp(64, viewportWidth * 0.058, 70);
        }

        return node.open
            ? clamp(94, viewportWidth * 0.08, 104)
            : clamp(84, viewportWidth * 0.072, 94);
    }

    function getEdgePoints(start, end) {
        const dx = ((end.x - start.x) / 100) * viewportWidth;
        const dy = ((end.y - start.y) / 100) * viewportHeight;
        const distance = Math.max(Math.hypot(dx, dy), 1);
        const unitX = dx / distance;
        const unitY = dy / distance;
        const getRadiusScale = (node) => {
            if (node.kind !== "person") {
                return 0.5;
            }

            const depth =
                node.motionDepth ?? (node.breadcrumb ? 1 : 0);
            return 0.34 * (1 - depth * 0.1);
        };
        const startScale = getRadiusScale(start);
        const endScale = getRadiusScale(end);
        const startRadius =
            (start.motionDiameter ?? getNodeDiameter(start)) * startScale;
        const endRadius =
            (end.motionDiameter ?? getNodeDiameter(end)) * endScale;

        return {
            x1: start.x + (unitX * startRadius * 100) / viewportWidth,
            y1: start.y + (unitY * startRadius * 100) / viewportHeight,
            x2: end.x - (unitX * endRadius * 100) / viewportWidth,
            y2: end.y - (unitY * endRadius * 100) / viewportHeight,
        };
    }

    function animateGraphTo(nextNodes, nextEdges, oldEdges) {
        if (!renderedNodes.length) {
            renderedNodes = nextNodes;
            renderedEdges = nextEdges;
            return;
        }

        window.cancelAnimationFrame(animationFrame);

        const currentById = new Map(
            renderedNodes.map((node) => [node.id, node]),
        );
        const oldEdgeIds = new Set(
            oldEdges.map((edge) => `${edge.source}:${edge.target}`),
        );
        const nextIds = new Set(nextNodes.map((node) => node.id));
        const nextEdgeIds = new Set(
            nextEdges.map((edge) => `${edge.source}:${edge.target}`),
        );
        const nextCenter = nextNodes.find(
            (node) => node.id === activeCategoryId,
        );
        const starts = new Map();
        const enteringCategory = view === "category";

        nextNodes.forEach((node) => {
            const current = currentById.get(node.id);
            const origin =
                current ??
                (node.kind === "item" && nextCenter
                    ? nextCenter
                    : node.kind !== "person"
                      ? getGraphOrigin()
                      : node);

            starts.set(node.id, {
                x: origin.x,
                y: origin.y,
                opacity: current ? 1 : 0,
                depth:
                    current?.motionDepth ?? (current?.breadcrumb ? 1 : 0),
                diameter:
                    current?.motionDiameter ??
                    getNodeDiameter(current ?? origin),
            });
        });

        const exitingNodes = renderedNodes
            .filter((node) => !nextIds.has(node.id))
            .map((node) => ({ ...node, exiting: true }));
        const exitingEdges = oldEdges
            .filter((edge) => !nextEdgeIds.has(`${edge.source}:${edge.target}`))
            .map((edge) => ({
                ...edge,
                exiting: true,
                fixedStart: enteringCategory
                    ? currentById.get(edge.source)
                    : undefined,
                fixedEnd: enteringCategory
                    ? currentById.get(edge.target)
                    : undefined,
            }));
        const startedAt = performance.now();
        const duration = 860;

        const tick = (now) => {
            const rawProgress = Math.min(1, (now - startedAt) / duration);
            const movingNodes = nextNodes.map((node) => {
                const start = starts.get(node.id);
                const itemOrder = Math.max(
                    0,
                    ((node.delay ?? 120) - 120) / 75,
                );
                const categoryOrder = Math.max(0, (node.delay ?? 0) / 70);
                let motionStart = 0;
                let motionEnd = 0.7;

                if (enteringCategory && start.opacity === 0) {
                    motionStart = 0.15 + itemOrder * 0.035;
                    motionEnd = 0.8 + itemOrder * 0.018;
                } else if (!enteringCategory) {
                    if (node.kind === "person") {
                        motionStart = 0.2;
                        motionEnd = 0.82;
                    } else if (start.opacity === 1) {
                        motionStart = 0.2;
                        motionEnd = 0.82;
                    } else {
                        motionStart = 0.42 + categoryOrder * 0.025;
                        motionEnd = 1;
                    }
                }

                const nodeProgress = progressBetween(
                    rawProgress,
                    motionStart,
                    motionEnd,
                );
                const progress = easeInOut(nodeProgress);

                return {
                    ...node,
                    x: start.x + (node.x - start.x) * progress,
                    y: start.y + (node.y - start.y) * progress,
                    motionDiameter:
                        start.diameter +
                        (getNodeDiameter(node) - start.diameter) * progress,
                    motionDepth:
                        start.depth +
                        ((node.breadcrumb ? 1 : 0) - start.depth) * progress,
                    motionOpacity:
                        start.opacity +
                        (1 - start.opacity) * easeInOut(nodeProgress),
                };
            });
            const fadingNodes = exitingNodes.map((node) => {
                if (!enteringCategory && node.kind === "item") {
                    const collapseProgress = easeInOut(
                        progressBetween(rawProgress, 0, 0.52),
                    );

                    return {
                        ...node,
                        x: node.x + (50 - node.x) * collapseProgress,
                        y: node.y + (50 - node.y) * collapseProgress,
                        motionOpacity:
                            1 -
                            easeInOut(
                                progressBetween(rawProgress, 0.12, 0.52),
                            ),
                    };
                }

                return {
                    ...node,
                    motionOpacity:
                        1 -
                        easeInOut(
                            progressBetween(rawProgress, 0, 0.28),
                        ),
                };
            });
            const fadingEdges = exitingEdges.map((edge) => ({
                ...edge,
                motionOpacity:
                    1 -
                    easeInOut(
                        progressBetween(
                            rawProgress,
                            enteringCategory ? 0 : 0.18,
                            enteringCategory ? 0.28 : 0.52,
                        ),
                    ),
            }));
            const movingEdges = nextEdges.map((edge) => {
                if (oldEdgeIds.has(`${edge.source}:${edge.target}`)) {
                    return { ...edge, motionOpacity: 1 };
                }

                const targetNode = nextNodes.find(
                    (node) => node.id === edge.target,
                );
                const itemOrder = Math.max(
                    0,
                    ((targetNode?.delay ?? 120) - 120) / 75,
                );
                const categoryOrder = Math.max(
                    0,
                    (targetNode?.delay ?? 0) / 70,
                );
                const edgeStart = enteringCategory
                    ? 0.18 + itemOrder * 0.035
                    : 0.55 + categoryOrder * 0.025;
                const edgeEnd = enteringCategory
                    ? 0.82 + itemOrder * 0.018
                    : 1;
                const edgeProgress = progressBetween(
                    rawProgress,
                    edgeStart,
                    edgeEnd,
                );

                return {
                    ...edge,
                    motionOpacity: easeInOut(edgeProgress),
                };
            });

            renderedNodes = [...movingNodes, ...fadingNodes];
            renderedEdges = [...movingEdges, ...fadingEdges];

            if (rawProgress < 1) {
                animationFrame = window.requestAnimationFrame(tick);
                return;
            }

            renderedNodes = nextNodes;
            renderedEdges = nextEdges;
            animationFrame = 0;
        };

        animationFrame = window.requestAnimationFrame(tick);
    }

    function startNavigation(update) {
        window.clearTimeout(transitionTimer);
        hoveredId = null;
        isNavigating = true;
        update();
        transitionTimer = window.setTimeout(() => {
            isNavigating = false;
        }, 860);
    }

    function enterCategory(categoryId) {
        startNavigation(() => {
            returnFocusId = categoryId;
            activeCategoryId = categoryId;
            selectedId = categoryId;
            view = "category";
        });
        focusElement("category-detail-title");
    }

    function returnToOverview() {
        const focusId = returnFocusId;
        startNavigation(() => {
            view = "overview";
            activeCategoryId = null;
            selectedId = person.id;
            returnFocusId = null;
        });
        if (focusId) {
            focusElement(`node-${focusId}`);
        }
    }

    function selectNode(node) {
        if (node.kind === "category" && view === "overview") {
            enterCategory(node.id);
            return;
        }

        if (node.kind === "person") {
            if (view === "category") {
                returnToOverview();
            }
            selectedId = person.id;
            return;
        }

        selectedId = node.id;
    }

    function focusElement(id) {
        requestAnimationFrame(() => {
            document.getElementById(id)?.focus();
        });
    }

    function handleKeydown(event) {
        if (event.key === "Escape" && view === "category") {
            event.preventDefault();
            returnToOverview();
        }
    }

    function edgeIsActive(edge) {
        if (hoveredId) {
            return edge.source === hoveredId || edge.target === hoveredId;
        }

        if (selectedId === person.id) {
            return false;
        }

        return (
            edge.target === selectedId ||
            (view === "category" &&
                edge.source === person.id &&
                edge.target === activeCategoryId)
        );
    }

    function nodeIsConnected(node) {
        if (selectedId === person.id || node.id === selectedId) {
            return false;
        }

        return graphEdges.some(
            (edge) =>
                (edge.source === node.id || edge.target === node.id) &&
                (edge.source === selectedId || edge.target === selectedId),
        );
    }

    function getNodeStyle(node) {
        const accent = "#c7634a";
        const opacity =
            node.motionOpacity === undefined
                ? ""
                : ` opacity: ${node.motionOpacity};`;
        const diameter =
            node.motionDiameter === undefined
                ? ""
                : ` --node-size: ${node.motionDiameter}px;`;
        const depth = node.motionDepth ?? (node.breadcrumb ? 1 : 0);
        const iconOpacity = 1;
        const iconScale = 1 - depth * 0.1;
        return `--x: ${node.x}%; --y: ${node.y}%; --accent: ${accent}; --delay: ${node.delay}ms; --icon-opacity: ${iconOpacity}; --icon-scale: ${iconScale};${diameter}${opacity}`;
    }

    function getEdgeStyle(edge) {
        const motionOpacity = edge.motionOpacity ?? 1;
        const isOpenBranch =
            view === "category" &&
            selectedId === activeCategoryId &&
            edge.source === activeCategoryId;
        const emphasis =
            selectedId !== person.id && !edgeIsActive(edge)
                ? isOpenBranch
                    ? 0.72
                    : 0.12
                : 1;

        const edgeColor = "rgba(199, 99, 74, .42)";

        return `--edge-color: ${edgeColor}; opacity: ${motionOpacity * emphasis};`;
    }

    $: activeCategory = categories.find(
        (category) => category.id === activeCategoryId,
    );
    $: graphNodes =
        (viewportWidth,
        viewportHeight,
        view === "overview" ? getOverviewNodes() : getCategoryNodes());
    $: graphEdges =
        view === "overview"
            ? categories.map((category) => ({
                  source: person.id,
                  target: category.id,
              }))
            : [
                  { source: person.id, target: activeCategoryId },
                  ...(activeCategory?.items ?? []).map((item) => ({
                      source: activeCategoryId,
                      target: item.id,
                  })),
              ];
    $: visibleEdges = renderedNodes.length
        ? renderedEdges.map((edge) => {
              const start = edge.fixedStart ?? getRenderedNode(edge.source);
              const end = edge.fixedEnd ?? getRenderedNode(edge.target);

              return {
                  ...edge,
                  start,
                  end,
                  points: getEdgePoints(start, end),
              };
          })
        : [];
    $: graphSignature = graphNodes
        .map((node) => `${node.id}:${node.x}:${node.y}`)
        .join("|");
    $: if (graphSignature !== layoutSignature) {
        layoutSignature = graphSignature;
        animateGraphTo(graphNodes, graphEdges, previousEdges);
        previousEdges = graphEdges;
    }
    $: selectedNode = getNode(selectedId);
</script>

<svelte:window onkeydown={handleKeydown} />
<svelte:head>
    <title>Luis Mauel — ScopeTV1</title>
    <meta
        name="description"
        content="Luis Mauel — ScopeTV1. Make the concept reality."
    />
</svelte:head>

<main
    id="top"
    class:view-category={view === "category"}
    style={`--scene-opacity: ${0.14 + scrollProgress * 0.86}; --scene-scale: ${0.86 + scrollProgress * 0.14}; --scene-y: ${(1 - scrollProgress) * 34}px;`}
>
    <nav class="topbar" aria-label="Main navigation">
        <div class="brand-lockup">
            <div class="wordmark">
                <a class="wordmark-name" href="#top" aria-label="Luis Mauel home">
                    <span>Luis</span> Mauel
                </a>
                <a
                    class="wordmark-handle"
                    href="https://github.com/ScopeTV1"
                    target="_blank"
                    rel="noreferrer"
                    aria-label="Visit ScopeTV1 on GitHub"
                    >@ScopeTV1</a
                >
            </div>
        </div>
        <div class="nav-links">
            <a href="#graph">Graph</a>
            <a
                class="nav-contact"
                href="https://be.linkedin.com/in/luismauel"
                target="_blank"
                rel="noreferrer"
                >LinkedIn <span>↗</span></a
            >
            <a
                class="nav-contact"
                href="https://github.com/ScopeTV1"
                target="_blank"
                rel="noreferrer"
                >GitHub <span>↗</span></a
            >
        </div>
    </nav>

    <section class="intro-section" aria-labelledby="intro-title">
        <h1 id="intro-title">
            Make the<br /><span class="hero-concept-line"><em>concept</em> reality.</span>
        </h1>
        <a
            class="scroll-cue"
            href="#graph"
            aria-label="Scroll to explore the graph"
        >
            <span>Scroll to explore</span>
            <span class="scroll-cue-arrow">↓</span>
        </a>
    </section>

    <section class="graph-section" id="graph" aria-labelledby="graph-title">
        <div
            class="graph-stage"
            class:view-category={view === "category"}
            aria-label="Interactive map of ScopeTV1's skills, tools, projects, and working contexts"
        >
            <h2 class="sr-only" id="graph-title">Explore the connections</h2>

            <div class="graph-canvas" class:is-navigating={isNavigating}>
                <svg
                    class="graph-lines"
                    viewBox="0 0 100 100"
                    preserveAspectRatio="none"
                    aria-hidden="true"
                >
                    {#each visibleEdges as edge}
                        <line
                            style={getEdgeStyle(edge)}
                            class:exiting={edge.exiting}
                            class:active={edgeIsActive(edge)}
                            class:dimmed={selectedId !== person.id &&
                                !edgeIsActive(edge)}
                            x1={edge.points.x1}
                            y1={edge.points.y1}
                            x2={edge.points.x2}
                            y2={edge.points.y2}
                        ></line>
                    {/each}
                </svg>

                <div
                    class="graph-nodes"
                    aria-label={view === "overview"
                        ? "Choose a category"
                        : `Choose an item from ${activeCategory?.label}`}
                >
                    {#each renderedNodes as node (node.id)}
                        <button
                            id={`node-${node.id}`}
                            class="graph-node {node.kind} {node.id}"
                            class:logo-node={node.logos?.length}
                            class:logo-pair={node.logos?.length > 1}
                            class:active={selectedId === node.id}
                            class:connected={nodeIsConnected(node)}
                            class:open={node.open}
                            class:breadcrumb={node.breadcrumb}
                            class:exiting={node.exiting}
                            style={getNodeStyle(node)}
                            aria-label={node.kind === "category"
                                ? `Open ${node.label} category`
                                : node.kind === "person"
                                  ? "Return to ScopeTV1 overview"
                                  : `Show ${node.label}`}
                            aria-pressed={selectedId === node.id}
                            aria-expanded={node.kind === "category"
                                ? view === "category"
                                : undefined}
                            aria-controls={node.kind === "category"
                                ? "node-detail"
                                : undefined}
                            aria-hidden={node.exiting ? "true" : undefined}
                            tabindex={node.exiting ? -1 : undefined}
                            onpointerenter={() => (hoveredId = node.id)}
                            onpointerleave={() => (hoveredId = null)}
                            onfocus={() => (hoveredId = node.id)}
                            onblur={() => (hoveredId = null)}
                            onclick={() => selectNode(node)}
                        >
                            {#if node.kind === "person"}
                                <span class="person-icon" aria-hidden="true">
                                    <svg
                                        viewBox="0 0 48 48"
                                        role="presentation"
                                    >
                                        <circle cx="24" cy="15" r="7"></circle>
                                        <path
                                            d="M11 39c1.7-8.2 6.1-12.3 13-12.3S35.3 30.8 37 39"
                                        ></path>
                                    </svg>
                                </span>
                            {:else if node.logos?.length}
                                <span class="tool-logo-lockup" aria-hidden="true">
                                    {#each node.logos as logo}
                                        <img class="tool-logo" src={logo} alt="" />
                                    {/each}
                                </span>
                            {:else}
                                <span class="node-label">{node.label}</span>
                            {/if}
                        </button>
                    {/each}
                </div>

                {#if view === "category"}
                    <button
                        class="back-control"
                        type="button"
                        onclick={returnToOverview}
                        aria-controls="graph"
                    >
                        <span aria-hidden="true">←</span> Overview
                    </button>
                {/if}

                {#if view === "category"}
                    <aside
                        class="graph-detail"
                        id="node-detail"
                        aria-live="polite"
                        aria-label="Selected node details"
                    >
                        <div class="detail-label">
                            <span
                                class="detail-dot"
                                style="--accent: #c7634a"
                            ></span>
                            {selectedNode.kind === "category"
                                ? "Thread"
                                : "Selected"}
                        </div>
                        <div class="detail-copy">
                            <h2 id="category-detail-title" tabindex="-1">
                                {selectedNode.label}
                            </h2>
                            <p>{selectedNode.description}</p>
                        </div>
                    </aside>
                {/if}
            </div>
        </div>
    </section>

    <footer class="site-footer">
        <span>© 2026 Luis Mauel</span>
        <span class="footer-note">Designed and built with intention.</span>
        <div class="footer-links">
            <a
                href="https://be.linkedin.com/in/luismauel"
                target="_blank"
                rel="noreferrer">LinkedIn ↗</a
            >
            <a
                href="https://github.com/ScopeTV1"
                target="_blank"
                rel="noreferrer">GitHub ↗</a
            >
        </div>
    </footer>
</main>
