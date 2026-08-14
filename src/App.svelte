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
            id: "projects",
            label: "Projects",
            eyebrow: "What I make",
            description:
                "Selected work where ideas become useful, working experiences.",
            kind: "category",
            accent: "#c7634a",
            symbol: "01",
            items: [
                {
                    id: "scopetv1",
                    label: "ScopeTV1",
                    eyebrow: "Interactive portfolio",
                    description:
                        "A responsive portfolio built as a navigable map of projects, experience, and engineering practice.",
                    kind: "item",
                    showOnOverview: true,
                    logos: [
                        "https://www.svgrepo.com/show/354416/svelte-icon.svg",
                    ],
                    tags: ["Svelte", "Vite", "Responsive UI"],
                },
            ],
        },
        {
            id: "background",
            label: "Background",
            eyebrow: "Where I grew",
            description:
                "The academic and professional environments that shaped how I think and build.",
            kind: "category",
            accent: "#8b7653",
            symbol: "02",
            items: [
                {
                    id: "universities",
                    label: "Universities",
                    eyebrow: "Academic context",
                    description:
                        "The places where I developed foundations, explored ideas, and learned to connect disciplines.",
                    kind: "item",
                    showOnOverview: true,
                    icon: "university",
                    tags: ["Learning", "Research", "Foundations"],
                },
                {
                    id: "companies",
                    label: "Companies",
                    eyebrow: "Professional context",
                    description:
                        "The teams and organizations where I applied those ideas to practical work and shared outcomes.",
                    kind: "item",
                    showOnOverview: true,
                    icon: "company",
                    tags: ["Teams", "Delivery", "Collaboration"],
                },
            ],
        },
        {
            id: "engineering",
            label: "Engineering",
            eyebrow: "How I build",
            description:
                "The technical practices I use to move from a concept to something reliable and useful.",
            kind: "category",
            accent: "#647b60",
            symbol: "03",
            items: [
                {
                    id: "development",
                    label: "Develop",
                    eyebrow: "Interfaces and applications",
                    description:
                        "Building clear, responsive products with SvelteKit, React, HTML, CSS, Java, and Python.",
                    kind: "item",
                    showOnOverview: true,
                    tags: ["SvelteKit", "React", "Java", "Python"],
                    tools: [
                        {
                            id: "sveltekit",
                            label: "SvelteKit",
                            description:
                                "Fast, focused web experiences built with SvelteKit.",
                            kind: "tool",
                            logos: [
                                "https://www.svgrepo.com/show/354416/svelte-icon.svg",
                            ],
                        },
                        {
                            id: "react",
                            label: "React",
                            description:
                                "Component-driven interfaces for flexible, interactive products.",
                            kind: "tool",
                            logos: [
                                "https://www.svgrepo.com/show/354259/react.svg",
                            ],
                        },
                        {
                            id: "html-css",
                            label: "HTML / CSS",
                            description:
                                "Structure, typography, and visual systems for the web.",
                            kind: "tool",
                            logos: [
                                "https://www.svgrepo.com/show/452228/html-5.svg",
                                "https://www.svgrepo.com/show/452185/css-3.svg",
                            ],
                        },
                    ],
                },
                {
                    id: "data-ai",
                    label: "Data + AI",
                    eyebrow: "Patterns and intelligence",
                    description:
                        "Turning information into insight with SQL, Microsoft Fabric, PySpark, machine learning, and AI-assisted workflows.",
                    kind: "item",
                    showOnOverview: true,
                    tags: ["SQL", "Fabric", "PySpark", "AI / ML"],
                    tools: [
                        {
                            id: "microsoft-fabric",
                            label: "Microsoft Fabric",
                            description:
                                "An analytics platform for bringing data, engineering, and insight together.",
                            kind: "tool",
                            logos: ["/logos/microsoft-fabric-color.svg?v=2"],
                        },
                        {
                            id: "sql",
                            label: "SQL",
                            description:
                                "Precise questions, useful structure, and reliable data work.",
                            kind: "tool",
                            logos: [
                                "https://www.svgrepo.com/show/331760/sql-database-generic.svg",
                            ],
                        },
                        {
                            id: "ai-ml",
                            label: "AI / ML",
                            description:
                                "Models and AI-assisted workflows that turn patterns into useful signals.",
                            kind: "tool",
                            logos: [
                                "https://www.svgrepo.com/show/306500/openai.svg",
                            ],
                        },
                    ],
                },
                {
                    id: "ship",
                    label: "Ship",
                    eyebrow: "Delivery and reliability",
                    description:
                        "Using Git, GitHub, testing, CI/CD, and deployment workflows to move changes safely into production.",
                    kind: "item",
                    showOnOverview: true,
                    tags: ["GitHub", "Testing", "CI/CD", "Deployment"],
                    tools: [
                        {
                            id: "github",
                            label: "GitHub",
                            description:
                                "Versioned collaboration and a dependable home for delivery workflows.",
                            kind: "tool",
                            logos: [
                                "https://www.svgrepo.com/show/353780/github-icon.svg",
                            ],
                        },
                        {
                            id: "ci-cd",
                            label: "CI/CD",
                            description:
                                "Automated checks and delivery pipelines that keep releases repeatable.",
                            kind: "tool",
                            icon: "cicd",
                        },
                        {
                            id: "testing",
                            label: "Testing",
                            description:
                                "Feedback loops that catch regressions before users do.",
                            kind: "tool",
                            icon: "testing",
                        },
                        {
                            id: "deployment",
                            label: "Deployment",
                            description:
                                "Moving tested changes into production with a clear release path.",
                            kind: "tool",
                            icon: "deployment",
                        },
                    ],
                },
            ],
        },
    ];

    let view = "overview";
    let activeCategoryId = null;
    let focusedItemId = null;
    let selectedId = person.id;
    let returnFocusId = null;
    let viewportWidth = 1200;
    let viewportHeight = 800;
    let hasCoarsePointer = false;
    let scrollProgress = 0;
    let footerProgress = 0;
    let isNavigating = false;
    let hoveredId = null;
    let transitionTimer;
    let renderedNodes = [];
    let renderedEdges = [];
    let layoutSignature = "";
    let previousEdges = [];
    let animationFrame = 0;
    let scrollAnimationFrame = 0;
    let cancelActiveScroll = null;

    onMount(() => {
        const updateViewport = () => {
            viewportWidth = window.innerWidth;
            viewportHeight = window.innerHeight;
            hasCoarsePointer = window.matchMedia("(pointer: coarse)").matches;
        };

        const updateScrollProgress = () => {
            const graphSection = document.getElementById("graph");
            const footer = document.getElementById("site-footer");
            if (!graphSection) {
                return;
            }

            const revealStart = window.innerHeight * 0.27;
            const revealDistance = Math.max(window.innerHeight * 0.65, 1);
            const progress =
                (revealStart - graphSection.getBoundingClientRect().top) /
                revealDistance;
            scrollProgress = Math.min(1, Math.max(0, progress));

            if (footer) {
                const footerRect = footer.getBoundingClientRect();
                const revealDistance = Math.max(footerRect.height, 1);
                const revealProgress =
                    (window.innerHeight - footerRect.top) / revealDistance;
                footerProgress = Math.min(1, Math.max(0, revealProgress));
            }
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
            cancelActiveScroll?.();
        };
    });

    $: isPhoneViewport =
        viewportWidth <= 640 ||
        (hasCoarsePointer && viewportWidth <= 960 && viewportHeight <= 520);
    $: isShortViewport = isPhoneViewport && viewportHeight <= 520;

    function getGraphOrigin() {
        return {
            x: 50,
            y:
                view === "category" && isPhoneViewport
                    ? isShortViewport
                        ? 43
                        : 46
                    : 50,
        };
    }

    function getGraphDimensions() {
        const graphStage =
            typeof document === "undefined"
                ? null
                : document.querySelector(".graph-stage");

        return {
            width: Math.max(
                Math.min(graphStage?.clientWidth ?? viewportWidth, 1280),
                320,
            ),
            height: Math.max(graphStage?.clientHeight ?? viewportHeight, 480),
        };
    }

    function getRadialRadii(scale = 1) {
        const { width, height } = getGraphDimensions();

        if (isPhoneViewport) {
            const radius =
                Math.min(
                    width * 0.28,
                    height * (isShortViewport ? 0.175 : 0.185),
                    155,
                ) * scale;

            return {
                radiusX: (radius / width) * 100,
                radiusY: (radius / height) * 100,
            };
        }

        const radius = Math.min(width * 0.175, height * 0.205) * scale;

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

    function positionAtAngle(
        angle,
        radiusX,
        radiusY,
        origin = { x: 50, y: 50 },
    ) {
        return {
            x: origin.x + Math.cos(angle) * radiusX,
            y: origin.y + Math.sin(angle) * radiusY,
        };
    }

    function positionAroundNode(
        node,
        index,
        total,
        radius,
        startAngle = -Math.PI / 2,
    ) {
        const { width, height } = getGraphDimensions();
        const angle = startAngle + (index * Math.PI * 2) / total;

        return {
            x: node.x + (Math.cos(angle) * radius * 100) / width,
            y: node.y + (Math.sin(angle) * radius * 100) / height,
        };
    }

    function constrainPosition(position, radius, margin = 8) {
        const { width, height } = getGraphDimensions();
        const insetX = ((radius + margin) * 100) / width;
        const insetY = ((radius + margin) * 100) / height;

        return {
            x: clamp(insetX, position.x, 100 - insetX),
            y: clamp(insetY, position.y, 100 - insetY),
        };
    }

    function getOverviewPreviewRadii() {
        const { width, height } = getGraphDimensions();
        const { radiusX } = getRadialRadii();
        const categoryRadius = (radiusX / 100) * width;
        const gap = isPhoneViewport
            ? viewportWidth <= 360
                ? 56
                : 60
            : clamp(76, width * 0.066, 84);
        const radius = Math.min(
            categoryRadius + gap,
            width * 0.47,
            height * 0.42,
        );

        return {
            radiusX: (radius / width) * 100,
            radiusY: (radius / height) * 100,
        };
    }

    function getOverviewNodes() {
        const origin = getGraphOrigin();
        const { radiusX, radiusY } = getRadialRadii();
        const previewRadii = getOverviewPreviewRadii();
        const startAngle = -Math.PI / 2;
        const categoryNodes = categories.map((category, index) => {
            const overviewAngle =
                startAngle + (index * Math.PI * 2) / categories.length;

            return {
                ...category,
                ...origin,
                ...positionAtAngle(
                    overviewAngle,
                    radiusX,
                    radiusY,
                    origin,
                ),
                overviewAngle,
                delay: index * 70,
            };
        });
        const previewNodes = categoryNodes.flatMap((category, categoryIndex) => {
            const previewItems = category.items.filter(
                (item) => item.showOnOverview,
            );
            const angleStep =
                category.id === "engineering"
                    ? 0.46
                    : isPhoneViewport
                      ? 0.24
                      : 0.2;
            const edgeBias =
                categoryIndex === 1
                    ? 0.1
                    : categoryIndex === 2
                      ? isPhoneViewport
                          ? -0.25
                          : -0.2
                      : 0;

            return previewItems.map((item, itemIndex) => {
                const angleOffset =
                    (itemIndex - (previewItems.length - 1) / 2) * angleStep;
                const previewPosition = constrainPosition(
                    positionAtAngle(
                        category.overviewAngle + edgeBias + angleOffset,
                        previewRadii.radiusX,
                        previewRadii.radiusY,
                        origin,
                    ),
                    isPhoneViewport ? 21 : 27,
                    12,
                );

                return {
                    ...item,
                    parentId: category.id,
                    preview: true,
                    ...previewPosition,
                    delay: 210 + categoryIndex * 90 + itemIndex * 45,
                };
            });
        });
        const overviewToolNodes = previewNodes.flatMap((item, itemIndex) =>
            (item.tools ?? []).map((tool, toolIndex) => ({
                ...tool,
                parentId: item.id,
                overviewTool: true,
                ...constrainPosition(
                    positionAroundNode(
                        item,
                        toolIndex,
                        item.tools.length,
                        isPhoneViewport ? 29 : 35,
                        -Math.PI / 2 + itemIndex * 0.31,
                    ),
                    isPhoneViewport ? 8 : 10,
                ),
                delay: 520 + itemIndex * 75 + toolIndex * 30,
            })),
        );

        return [
            { ...person, ...origin, delay: 0 },
            ...categoryNodes,
            ...previewNodes,
            ...overviewToolNodes,
        ];
    }

    function getCategoryNodes() {
        if (!activeCategory) {
            return [{ ...person, ...getGraphOrigin(), delay: 0 }];
        }

        const origin = getGraphOrigin();
        const focusedItem = activeCategory.items.find(
            (item) => item.id === focusedItemId && item.tools?.length,
        );
        const hasToolOrbits = activeCategory.items.some(
            (item) => item.tools?.length,
        );
        const { radiusX, radiusY } = getRadialRadii(
            focusedItem ? 1.15 : hasToolOrbits ? 1 : 0.9,
        );
        let itemNodes;

        if (focusedItem) {
            const siblings = activeCategory.items.filter(
                (item) => item.id !== focusedItem.id,
            );
            const siblingNodes = siblings.map((item, index) => {
                const angle =
                    siblings.length === 1
                        ? 0
                        : -Math.PI / 4 +
                          (index * Math.PI) / 2 / (siblings.length - 1);

                return {
                    ...item,
                    parentId: activeCategory.id,
                    ...constrainPosition(
                        positionAtAngle(
                            angle,
                            radiusX,
                            radiusY,
                            origin,
                        ),
                        isPhoneViewport ? 29 : 38,
                        12,
                    ),
                    contextItem: true,
                    delay: 180 + index * 90,
                };
            });

            itemNodes = [
                {
                    ...focusedItem,
                    ...origin,
                    parentId: activeCategory.id,
                    focused: true,
                    delay: 0,
                },
                ...siblingNodes,
            ];
        } else {
            itemNodes = activeCategory.items.map((item, index) => ({
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
            }));
        }

        const toolNodes = itemNodes.flatMap((item, itemIndex) => {
            const isFocusedBranch = item.id === focusedItem?.id;
            const toolOrbitRadius = isFocusedBranch
                ? isPhoneViewport
                    ? 56
                    : 72
                : focusedItem
                  ? isPhoneViewport
                      ? 40
                      : 48
                  : isPhoneViewport
                    ? viewportWidth <= 360
                        ? 46
                        : 48
                    : 56;

            return (item.tools ?? []).map((tool, toolIndex) => ({
                ...tool,
                parentId: item.id,
                backgroundTool: Boolean(focusedItem && !isFocusedBranch),
                ...constrainPosition(
                    positionAroundNode(
                        item,
                        toolIndex,
                        item.tools.length,
                        toolOrbitRadius,
                        -Math.PI / 2 + itemIndex * 0.28,
                    ),
                    isPhoneViewport ? 12 : 14,
                    8,
                ),
                delay: 280 + itemIndex * 110 + toolIndex * 38,
            }));
        });

        return [
            {
                ...person,
                x: focusedItem ? (isPhoneViewport ? 9 : 12) : isPhoneViewport ? 10 : 15,
                y: focusedItem ? origin.y - (isPhoneViewport ? 12 : 15) : origin.y,
                breadcrumb: true,
                delay: 0,
            },
            {
                ...activeCategory,
                ...(focusedItem
                      ? {
                          x: isPhoneViewport ? 12 : 13,
                          y: origin.y + (isPhoneViewport ? 13 : 16),
                      }
                    : origin),
                open: !focusedItem,
                context: Boolean(focusedItem),
                delay: 0,
            },
            ...itemNodes,
            ...toolNodes,
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
        if (isPhoneViewport) {
            if (node.overviewTool) {
                return 14;
            }

            if (node.backgroundTool) {
                return 20;
            }

            if (node.preview) {
                return viewportWidth <= 360 ? 36 : 40;
            }

            if (node.focused) {
                return 72;
            }

            if (node.context) {
                return 52;
            }

            if (node.kind === "tool") {
                return viewportWidth <= 360 ? 30 : 32;
            }

            if (node.kind === "person") {
                return node.breadcrumb ? 52 : 78;
            }

            if (node.kind === "item") {
                return viewportWidth <= 360 ? 50 : 54;
            }

            return node.open ? 74 : 68;
        }

        if (node.overviewTool) {
            return 18;
        }

        if (node.backgroundTool) {
            return 24;
        }

        if (node.focused) {
            return clamp(92, viewportWidth * 0.078, 100);
        }

        if (node.context) {
            return clamp(64, viewportWidth * 0.056, 70);
        }

        if (node.preview) {
            return clamp(44, viewportWidth * 0.038, 48);
        }

        if (node.kind === "tool") {
            return clamp(34, viewportWidth * 0.034, 38);
        }

        if (node.kind === "person") {
            return node.breadcrumb
                ? clamp(68, viewportWidth * 0.058, 76)
                : clamp(104, viewportWidth * 0.086, 114);
        }

        if (node.kind === "item") {
            return clamp(60, viewportWidth * 0.055, 66);
        }

        return node.open
            ? clamp(90, viewportWidth * 0.078, 98)
            : clamp(84, viewportWidth * 0.072, 92);
    }

    function getEdgePoints(start, end) {
        const { width, height } = getGraphDimensions();
        const dx = ((end.x - start.x) / 100) * width;
        const dy = ((end.y - start.y) / 100) * height;
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
            x1: start.x + (unitX * startRadius * 100) / width,
            y1: start.y + (unitY * startRadius * 100) / height,
            x2: end.x - (unitX * endRadius * 100) / width,
            y2: end.y - (unitY * endRadius * 100) / height,
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
            const parent = node.parentId
                ? nextNodes.find((candidate) => candidate.id === node.parentId)
                : null;
            const origin =
                current ??
                (parent ??
                (node.kind === "item" && nextCenter
                    ? nextCenter
                    : node.kind !== "person"
                      ? getGraphOrigin()
                      : node));

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

    function enterCategory(categoryId, selectedNodeId = categoryId) {
        const category = categories.find((item) => item.id === categoryId);
        const selectedItem = category?.items.find(
            (item) => item.id === selectedNodeId,
        );

        startNavigation(() => {
            returnFocusId = selectedNodeId;
            activeCategoryId = categoryId;
            focusedItemId = selectedItem?.tools?.length
                ? selectedItem.id
                : null;
            selectedId = selectedNodeId;
            view = "category";
        });
        focusElement("category-detail-title");
    }

    function returnToOverview() {
        const focusId = returnFocusId;
        startNavigation(() => {
            view = "overview";
            activeCategoryId = null;
            focusedItemId = null;
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

        if (node.preview && view === "overview" && node.parentId) {
            enterCategory(node.parentId, node.id);
            return;
        }

        if (
            view === "category" &&
            node.kind === "category" &&
            node.id === activeCategoryId &&
            focusedItemId
        ) {
            startNavigation(() => {
                focusedItemId = null;
                selectedId = activeCategoryId;
            });
            return;
        }

        if (view === "category" && node.kind === "item" && node.tools?.length) {
            startNavigation(() => {
                focusedItemId = node.id;
                selectedId = node.id;
            });
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

    function scrollToFooter(event) {
        event.preventDefault();
        const footer = document.getElementById("site-footer");
        if (!footer) {
            return;
        }

        if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) {
            footer.scrollIntoView({ block: "end" });
            return;
        }

        cancelActiveScroll?.();

        const start = window.scrollY;
        const maximum =
            document.documentElement.scrollHeight - window.innerHeight;
        const target = Math.min(
            maximum,
            Math.max(
                0,
                start + footer.getBoundingClientRect().bottom - window.innerHeight,
            ),
        );
        const distance = target - start;
        const duration = Math.min(
            3000,
            Math.max(2200, Math.abs(distance) * 1.5),
        );
        const startedAt = performance.now();

        const stop = () => {
            window.cancelAnimationFrame(scrollAnimationFrame);
            scrollAnimationFrame = 0;
            document.documentElement.classList.remove("is-slow-scrolling");
            window.removeEventListener("wheel", stop);
            window.removeEventListener("touchstart", stop);
            window.removeEventListener("keydown", stop);
            window.removeEventListener("resize", stop);
            if (cancelActiveScroll === stop) {
                cancelActiveScroll = null;
            }
        };

        const animate = (now) => {
            const progress = Math.min(1, (now - startedAt) / duration);
            window.scrollTo(0, start + distance * easeInOut(progress));

            if (progress < 1) {
                scrollAnimationFrame = window.requestAnimationFrame(animate);
            } else {
                stop();
            }
        };

        cancelActiveScroll = stop;
        document.documentElement.classList.add("is-slow-scrolling");
        window.addEventListener("wheel", stop, { passive: true });
        window.addEventListener("touchstart", stop, { passive: true });
        window.addEventListener("keydown", stop);
        window.addEventListener("resize", stop);
        scrollAnimationFrame = window.requestAnimationFrame(animate);
    }

    function handleKeydown(event) {
        if (event.key === "Escape" && view === "category") {
            event.preventDefault();
            if (focusedItemId) {
                startNavigation(() => {
                    focusedItemId = null;
                    selectedId = activeCategoryId;
                });
                return;
            }
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
        const selectionEmphasis =
            selectedId !== person.id && !edgeIsActive(edge)
                ? isOpenBranch
                    ? 0.86
                    : 0.58
                : 1;
        const hierarchyEmphasis = edge.overviewTool ? 0.52 : 1;

        const edgeColor = "rgba(199, 99, 74, .42)";

        return `--edge-color: ${edgeColor}; opacity: ${motionOpacity * selectionEmphasis * hierarchyEmphasis};`;
    }

    $: activeCategory = categories.find(
        (category) => category.id === activeCategoryId,
    );
    $: graphNodes =
        (viewportWidth,
        viewportHeight,
        activeCategoryId,
        focusedItemId,
        view === "overview" ? getOverviewNodes() : getCategoryNodes());
    $: graphEdges =
        view === "overview"
            ? [
                  ...categories.map((category) => ({
                      source: person.id,
                      target: category.id,
                  })),
                  ...categories.flatMap((category) =>
                      category.items
                          .filter((item) => item.showOnOverview)
                          .map((item) => ({
                              source: category.id,
                              target: item.id,
                          })),
                  ),
                  ...categories.flatMap((category) =>
                      category.items.flatMap((item) =>
                          (item.tools ?? []).map((tool) => ({
                              source: item.id,
                              target: tool.id,
                              overviewTool: true,
                          })),
                      ),
                  ),
              ]
            : [
                  { source: person.id, target: activeCategoryId },
                  ...(activeCategory?.items ?? []).map((item) => ({
                      source: activeCategoryId,
                      target: item.id,
                  })),
                  ...(activeCategory?.items ?? []).flatMap((item) =>
                      (item.tools ?? []).map((tool) => ({
                          source: item.id,
                          target: tool.id,
                      })),
                  ),
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
    $: sceneProgress = scrollProgress;
    $: easedFooterProgress = easeInOut(footerProgress);
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
    style={`--scene-opacity: ${sceneProgress}; --scene-scale: 1; --scene-y: 0; --scene-veil-opacity: 0; --footer-opacity: ${0.2 + easedFooterProgress * 0.8}; --footer-y: ${(1 - easedFooterProgress) * 12}px;`}
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
            href="#site-footer"
            onclick={scrollToFooter}
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
            aria-label="Interactive map of ScopeTV1's projects, background, and engineering practice"
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
                        ? "Choose a category or featured item"
                        : `Choose an item from ${activeCategory?.label}`}
                >
                    {#each renderedNodes as node (node.id)}
                        <button
                            id={`node-${node.id}`}
                            class="graph-node {node.kind} {node.id} {node.overviewTool
                                ? 'overview-tool'
                                : ''} {node.backgroundTool
                                ? 'background-tool'
                                : ''}"
                            class:logo-node={node.logos?.length}
                            class:logo-pair={node.logos?.length > 1}
                            class:preview={node.preview}
                            class:focused={node.focused}
                            class:context={node.context}
                            class:active={selectedId === node.id}
                            class:connected={nodeIsConnected(node)}
                            class:open={node.open}
                            class:breadcrumb={node.breadcrumb}
                            class:exiting={node.exiting}
                            style={getNodeStyle(node)}
                            title={node.kind === "tool" ? node.label : undefined}
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
                            aria-hidden={node.exiting || node.overviewTool
                                ? "true"
                                : undefined}
                            tabindex={node.exiting || node.overviewTool
                                ? -1
                                : undefined}
                            onpointerenter={() => (hoveredId = node.id)}
                            onpointerleave={() => (hoveredId = null)}
                            onfocus={() => (hoveredId = node.id)}
                            onblur={() => (hoveredId = null)}
                            onclick={() => selectNode(node)}
                        >
                            {#if node.kind === "person"}
                                <span class="person-icon" aria-hidden="true">
                                    <img
                                        class="person-avatar"
                                        src="/images/luis-mountain-avatar.png"
                                        alt=""
                                    />
                                </span>
                            {:else if node.logos?.length}
                                <span class="tool-logo-lockup" aria-hidden="true">
                                    {#each node.logos as logo}
                                        <img class="tool-logo" src={logo} alt="" />
                                    {/each}
                                </span>
                            {:else if node.icon}
                                <span class="node-symbol" aria-hidden="true">
                                    <svg viewBox="0 0 24 24">
                                        {#if node.icon === "university"}
                                            <path d="M3 8.5 12 4l9 4.5-9 4.5-9-4.5Z"></path>
                                            <path d="M6.5 10.5v5c3.5 2.4 7.5 2.4 11 0v-5"></path>
                                        {:else if node.icon === "company"}
                                            <path d="M5 21V5h9v16M14 10h5v11M3 21h18"></path>
                                            <path d="M8 8h3M8 12h3M8 16h3M16.5 13h.01M16.5 17h.01"></path>
                                        {:else if node.icon === "cicd"}
                                            <path d="M7 7h9l-2.5-2.5M17 17H8l2.5 2.5"></path>
                                            <path d="M18.5 8.5A7 7 0 0 1 17 17M5.5 15.5A7 7 0 0 1 7 7"></path>
                                        {:else if node.icon === "testing"}
                                            <circle cx="12" cy="12" r="8"></circle>
                                            <path d="m8.5 12 2.2 2.2 4.8-5"></path>
                                        {:else if node.icon === "deployment"}
                                            <path d="M12 16V4m0 0L8 8m4-4 4 4"></path>
                                            <path d="M5 14v5h14v-5"></path>
                                        {/if}
                                    </svg>
                                </span>
                            {:else}
                                <span class="node-label">{node.label}</span>
                            {/if}
                        </button>
                    {/each}
                </div>

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
                            {#if selectedNode.tags?.length}
                                <ul
                                    class="detail-tags"
                                    aria-label={`${selectedNode.label} tools and themes`}
                                >
                                    {#each selectedNode.tags as tag}
                                        <li>{tag}</li>
                                    {/each}
                                </ul>
                            {/if}
                        </div>
                    </aside>
                {/if}
            </div>
        </div>
    </section>

    <footer class="site-footer" id="site-footer">
        <span>© 2026 Luis Mauel</span>
        <span class="footer-note">Designed with intention.</span>
        <span class="footer-tech" aria-label="Built with Svelte">
            <span>Built with</span>
            <span class="footer-tech-tools" aria-hidden="true">
                <img
                    src="https://cdn.simpleicons.org/svelte/FF3E00"
                    alt=""
                />
            </span>
        </span>
    </footer>
</main>
