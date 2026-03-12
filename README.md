# guiasolar.github.io
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guía Práctica: Generación Distribuida en Argentina</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <!-- Chosen Palette: Eco-Tech Argentine (Slate backgrounds, Amber for solar/energy, Emerald for eco/success, Sky blue for neutral data) -->
    <!-- Application Structure Plan: Dashboard/Documentation hybrid. A fixed sidebar allows quick navigation between the 8 highly specific technical chapters. The main content area presents data using accordions for step-by-step processes, tabs for comparing residential vs. industrial setups, and an interactive calculator to make the sizing chapter practical. This structure is chosen because technical manuals are often linear and boring; breaking them into distinct, interactive modules improves comprehension and task-oriented reading. -->
    <!-- Visualization & Content Choices: 
         - Ch1-Ch5: Text with styled callout boxes -> Goal: Inform -> Clean HTML/CSS.
         - Ch2 (Steps): Vertical timeline (CSS) -> Goal: Process -> Shows sequential flow clearly.
         - Ch6 (Equipment): Tabs -> Goal: Compare -> Easy switching between Home/Industry without scrolling.
         - Ch7 (Suppliers): Grid of Cards -> Goal: Organize -> Easy to scan contact info.
         - Ch8 (Sizing): Interactive Calculator (JS) & Line Chart (Chart.js) -> Goal: Apply knowledge & Understand relationship -> Allows user to input their bill and see a result, chart explains the physical concept of solar generation vs consumption. NO SVG used (using HTML entities). 
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0fdfa',
                            100: '#ccfbf1',
                            500: '#14b8a6', // Teal
                            600: '#0d9488',
                            900: '#134e4a',
                        },
                        sun: {
                            400: '#fbbf24',
                            500: '#f59e0b',
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');
        
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; color: #334155; }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }

        /* Chart Container Requirements */
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 400px;
        }

        /* Timeline Styles */
        .timeline-item { border-left: 2px solid #cbd5e1; padding-left: 1.5rem; position: relative; padding-bottom: 2rem; }
        .timeline-item::before {
            content: ''; position: absolute; left: -0.4rem; top: 0; width: 0.75rem; height: 0.75rem;
            background-color: #f59e0b; border-radius: 50%;
        }
        .timeline-item:last-child { border-left: 2px solid transparent; }

        /* Hide sections by default */
        .content-section { display: none; }
        .content-section.active { display: block; animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .nav-item.active { background-color: #f0fdfa; color: #0d9488; border-right: 4px solid #0d9488; font-weight: 600; }
    </style>
</head>
<body class="flex h-screen overflow-hidden antialiased">

    <!-- Sidebar Navigation -->
    <aside class="w-64 bg-white border-r border-slate-200 flex flex-col h-full hidden md:flex flex-shrink-0 z-10 shadow-sm">
        <div class="p-6 border-b border-slate-100 flex items-center gap-3">
            <span class="text-3xl text-sun-500">&#9889;</span>
            <h1 class="text-xl font-bold text-slate-800 leading-tight">GenDist<br><span class="text-sm font-normal text-slate-500">Argentina</span></h1>
        </div>
        <nav class="flex-1 overflow-y-auto py-4" id="sidebarNav">
            <button onclick="showSection('sec-intro')" class="nav-item active w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">Inicio</button>
            <button onclick="showSection('sec-normativa')" class="nav-item w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">1. Normativa Vigente</button>
            <button onclick="showSection('sec-pasos')" class="nav-item w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">2. Guía de Registro</button>
            <button onclick="showSection('sec-instalador')" class="nav-item w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">3. Req. del Instalador</button>
            <button onclick="showSection('sec-equipamiento')" class="nav-item w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">4. Equipamiento Homologado</button>
            <button onclick="showSection('sec-seguridad')" class="nav-item w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">5. Seguridad Eléctrica</button>
            <button onclick="showSection('sec-listado')" class="nav-item w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">6. Listado de Materiales</button>
            <button onclick="showSection('sec-proveedores')" class="nav-item w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">7. Proveedores</button>
            <button onclick="showSection('sec-dimensionamiento')" class="nav-item w-full text-left px-6 py-3 text-slate-600 hover:bg-slate-50 transition-colors text-sm">8. Dimensionamiento (Calculadora)</button>
        </nav>
        <div class="p-4 border-t border-slate-100 text-xs text-slate-400 text-center">
            Guía Técnica Interactiva 2026
        </div>
    </aside>

    <!-- Mobile Header -->
    <div class="md:hidden fixed top-0 w-full bg-white border-b border-slate-200 z-20 px-4 py-3 flex justify-between items-center shadow-sm">
        <div class="flex items-center gap-2">
            <span class="text-2xl text-sun-500">&#9889;</span>
            <span class="font-bold text-slate-800">GenDist Arg</span>
        </div>
        <select id="mobileNav" onchange="showSection(this.value)" class="bg-slate-50 border border-slate-300 text-slate-700 text-sm rounded-lg focus:ring-brand-500 focus:border-brand-500 block p-2">
            <option value="sec-intro">Inicio</option>
            <option value="sec-normativa">1. Normativa</option>
            <option value="sec-pasos">2. Registro</option>
            <option value="sec-instalador">3. Instalador</option>
            <option value="sec-equipamiento">4. Homologación</option>
            <option value="sec-seguridad">5. Seguridad</option>
            <option value="sec-listado">6. Materiales</option>
            <option value="sec-proveedores">7. Proveedores</option>
            <option value="sec-dimensionamiento">8. Dimensionamiento</option>
        </select>
    </div>

    <!-- Main Content -->
    <main class="flex-1 overflow-y-auto w-full pt-16 md:pt-0 bg-slate-50 relative">
        <div class="max-w-5xl mx-auto p-6 md:p-10 pb-24">

            <!-- Intro Section -->
            <section id="sec-intro" class="content-section active">
                <div class="bg-white rounded-2xl p-8 shadow-sm border border-slate-100 text-center mt-10">
                    <span class="text-6xl text-sun-500 block mb-4">&#9728;&#65039;</span>
                    <h2 class="text-3xl font-bold text-slate-800 mb-4">Generación Distribuida en Argentina</h2>
                    <p class="text-slate-600 text-lg max-w-2xl mx-auto mb-8">
                        Bienvenido a la guía práctica interactiva. Aquí encontrarás toda la información técnica, legal y práctica necesaria para entender, proyectar e instalar sistemas solares fotovoltaicos conectados a la red bajo la normativa vigente, con especial foco en la Provincia de Buenos Aires.
                    </p>
                    <button onclick="showSection('sec-normativa')" class="bg-brand-600 hover:bg-brand-700 text-white font-semibold py-3 px-6 rounded-lg transition-colors shadow-sm">
                        Comenzar Guía &#8594;
                    </button>
                </div>
            </section>

            <!-- Chapter 1: Normativa -->
            <section id="sec-normativa" class="content-section">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 border-b pb-2 mb-4">1. Normativa Vigente (Nacional y PBA)</h2>
                    <p class="text-slate-600 mb-6">Esta sección detalla el marco legal que permite a los usuarios inyectar energía a la red eléctrica. Es fundamental entender el modelo de "Balance Neto de Facturación" (Net Billing) que rige en Argentina.</p>
                </div>

                <div class="grid md:grid-cols-2 gap-6">
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                        <div class="flex items-center gap-3 mb-4">
                            <span class="text-2xl text-blue-600">&#127985;</span>
                            <h3 class="text-lg font-bold text-slate-800">Nivel Nacional: Ley 27.424</h3>
                        </div>
                        <p class="text-sm text-slate-600 mb-4">Régimen de Fomento a la Generación Distribuida de Energía Renovable integrada a la Red Eléctrica Pública.</p>
                        <ul class="text-sm text-slate-700 space-y-2">
                            <li>&#10004; <strong>Figura Legal:</strong> Crea el "Usuario Generador".</li>
                            <li>&#10004; <strong>Esquema:</strong> Net Billing. La energía inyectada se compensa a un precio (precio estacional + transporte), que suele ser menor al precio de la energía consumida.</li>
                            <li>&#10004; <strong>Límites:</strong> La potencia a instalar no puede superar la potencia contratada por el usuario con la distribuidora.</li>
                        </ul>
                    </div>

                    <div class="bg-white p-6 rounded-xl shadow-sm border border-brand-200 bg-brand-50/30">
                        <div class="flex items-center gap-3 mb-4">
                            <span class="text-2xl text-brand-600">&#128205;</span>
                            <h3 class="text-lg font-bold text-slate-800">Provincia de Buenos Aires: Ley 15.325</h3>
                        </div>
                        <p class="text-sm text-slate-600 mb-4">Adhesión provincial a la Ley Nacional, reglamentada por la Resolución 236/2023 del Ministerio de Infraestructura.</p>
                        <ul class="text-sm text-slate-700 space-y-2">
                            <li>&#10004; <strong>Autoridad de Aplicación:</strong> Subsecretaría de Energía PBA y OCEBA.</li>
                            <li>&#10004; <strong>Beneficios Impositivos (PBA):</strong> Exención de impuestos provinciales (Ingresos Brutos, Sellos) sobre la inyección por 12 años.</li>
                            <li>&#10004; <strong>PROINGED:</strong> Foro provincial de promoción; financiamiento (FITBA) para entidades públicas, aunque la ley aplica a todos.</li>
                        </ul>
                    </div>
                </div>
            </section>

            <!-- Chapter 2: Registro -->
            <section id="sec-pasos" class="content-section">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 border-b pb-2 mb-4">2. Guía paso a paso: Registrar un Usuario Generador</h2>
                    <p class="text-slate-600 mb-6">El trámite se realiza íntegramente de manera digital a través de la Plataforma de Trámites a Distancia (TAD) o la plataforma específica de la distribuidora (ej. Edenor, Edesur, Edelap, Cooperativas). Requiere intervención de un instalador matriculado.</p>
                </div>

                <div class="bg-white p-8 rounded-xl shadow-sm border border-slate-200">
                    <div class="timeline-item">
                        <h4 class="font-bold text-slate-800">Paso 1: Solicitud de Reserva de Potencia (Trámite A)</h4>
                        <p class="text-sm text-slate-600 mt-1">El usuario (o el instalador apoderado) solicita a la distribuidora la factibilidad de conexión informando la potencia del inversor a instalar. Se adjuntan datos del suministro.</p>
                    </div>
                    <div class="timeline-item">
                        <h4 class="font-bold text-slate-800">Paso 2: Aprobación de la Distribuidora</h4>
                        <p class="text-sm text-slate-600 mt-1">La distribuidora analiza la red en la zona y aprueba (o rechaza con justificación técnica) la reserva de potencia. Tiene un plazo normado (usualmente 15-30 días).</p>
                    </div>
                    <div class="timeline-item">
                        <h4 class="font-bold text-slate-800">Paso 3: Instalación Física del Sistema</h4>
                        <p class="text-sm text-slate-600 mt-1">Una vez aprobada la reserva, el instalador calificado procede al montaje de paneles, inversor y tableros bajo normativa eléctrica.</p>
                    </div>
                    <div class="timeline-item">
                        <h4 class="font-bold text-slate-800">Paso 4: Solicitud de Certificado de Usuario Generador (Trámite B)</h4>
                        <p class="text-sm text-slate-600 mt-1">Finalizada la obra, se presenta la documentación técnica final (planos unifilares, certificados de equipos, encomienda del colegio profesional) para obtener el Certificado de Instalación.</p>
                    </div>
                    <div class="timeline-item">
                        <h4 class="font-bold text-slate-800">Paso 5: Cambio de Medidor y Contrato</h4>
                        <p class="text-sm text-slate-600 mt-1">Con el certificado, la distribuidora acude al domicilio a reemplazar el medidor tradicional por uno <strong>bidireccional</strong>. Se firma el Contrato de Generación Distribuida.</p>
                    </div>
                </div>
            </section>

            <!-- Chapter 3: Instalador -->
            <section id="sec-instalador" class="content-section">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 border-b pb-2 mb-4">3. Requerimientos para el Instalador</h2>
                    <p class="text-slate-600 mb-6">Para firmar los anexos técnicos frente a la distribuidora y la Secretaría de Energía, el instalador no puede ser un idóneo sin título; debe cumplir requisitos formales y legales.</p>
                </div>

                <div class="grid md:grid-cols-3 gap-6">
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                        <div class="text-3xl mb-3 text-slate-700">&#128196;</div>
                        <h3 class="font-bold text-slate-800 mb-2">1. Titulación</h3>
                        <p class="text-sm text-slate-600">Título habilitante en especialidad eléctrica o electromecánica (Ingeniero, Técnico). Debe tener incumbencias para proyectar y ejecutar instalaciones eléctricas.</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                        <div class="text-3xl mb-3 text-brand-600">&#127970;</div>
                        <h3 class="font-bold text-slate-800 mb-2">2. Matriculación</h3>
                        <p class="text-sm text-slate-600">Matrícula vigente en el colegio profesional correspondiente a la jurisdicción (ej. CIPBA, Colegio de Técnicos PBA, COPIME en CABA).</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                        <div class="text-3xl mb-3 text-sun-500">&#128188;</div>
                        <h3 class="font-bold text-slate-800 mb-2">3. AFIP y Seguros</h3>
                        <p class="text-sm text-slate-600">Inscripción en AFIP (Monotributo/Responsable Inscripto). Seguro de Responsabilidad Civil Comprensiva y ART/Seguro de Accidentes Personales para trabajo en altura.</p>
                    </div>
                </div>
            </section>

            <!-- Chapter 4: Homologación -->
            <section id="sec-equipamiento" class="content-section">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 border-b pb-2 mb-4">4. Equipamiento Homologado</h2>
                    <p class="text-slate-600 mb-6">No se puede conectar cualquier equipo a la red pública. El elemento crítico es el Inversor (o microinversor), que debe estar certificado para garantizar la seguridad de la red y evitar que inyecte energía si hay un corte de luz en el barrio.</p>
                </div>

                <div class="bg-blue-50 border-l-4 border-blue-500 p-4 mb-6">
                    <p class="text-sm text-blue-800"><strong>¿Qué revisa la distribuidora?</strong> Que el equipo figure en el listado oficial de la Subsecretaría de Energía o que cuente con los certificados IRAM/IEC vigentes ensayados por un laboratorio reconocido (ej. INTI, IADEV).</p>
                </div>

                <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
                    <table class="w-full text-left text-sm text-slate-600">
                        <thead class="bg-slate-50 border-b border-slate-200 text-slate-700">
                            <tr>
                                <th class="px-6 py-3">Componente</th>
                                <th class="px-6 py-3">Normativa / Certificación Exigida</th>
                                <th class="px-6 py-3">Razón principal</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-slate-100">
                            <tr>
                                <td class="px-6 py-4 font-semibold text-slate-800">Inversor On-Grid</td>
                                <td class="px-6 py-4">IRAM 2150-1-1 / IEC 62109 / IEC 61727</td>
                                <td class="px-6 py-4">Calidad de onda, armónicos y <strong>función anti-isla</strong> (debe apagarse si se corta la luz de la calle).</td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 font-semibold text-slate-800">Paneles Solares</td>
                                <td class="px-6 py-4">IEC 61215 / IEC 61730</td>
                                <td class="px-6 py-4">Seguridad eléctrica ante intemperie y garantía de degradación de potencia.</td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 font-semibold text-slate-800">Cables DC</td>
                                <td class="px-6 py-4">Norma TUV 2 PfG 1169 o similar</td>
                                <td class="px-6 py-4">Resistencia a rayos UV, doble aislación, aptos para intemperie extrema.</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </section>

            <!-- Chapter 5: Seguridad -->
            <section id="sec-seguridad" class="content-section">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 border-b pb-2 mb-4">5. Seguridad Eléctrica (Reglamento AEA 90364-7-712)</h2>
                    <p class="text-slate-600 mb-6">Una instalación fotovoltaica genera energía en Corriente Continua (DC) a voltajes letales (hasta 1000V). Las exigencias de protección son rigurosas.</p>
                </div>

                <div class="grid md:grid-cols-2 gap-6">
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                        <h3 class="font-bold text-slate-800 mb-3 text-lg flex items-center gap-2"><span class="text-red-500">&#9888;&#65039;</span> Lado Continua (DC - Paneles)</h3>
                        <ul class="space-y-3 text-sm text-slate-600">
                            <li class="flex gap-2"><span>&#10145;&#65039;</span> <div><strong>Fusibles cilíndricos DC:</strong> Requeridos si hay más de 2 strings en paralelo.</div></li>
                            <li class="flex gap-2"><span>&#10145;&#65039;</span> <div><strong>DPS DC (Descargador de Sobretensiones):</strong> Tipo II obligatorio para proteger el inversor de rayos.</div></li>
                            <li class="flex gap-2"><span>&#10145;&#65039;</span> <div><strong>Seccionador DC:</strong> Corte omnipolar bajo carga (suele venir integrado en buenos inversores).</div></li>
                        </ul>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                        <h3 class="font-bold text-slate-800 mb-3 text-lg flex items-center gap-2"><span class="text-blue-500">&#128268;</span> Lado Alterna (AC - Red)</h3>
                        <ul class="space-y-3 text-sm text-slate-600">
                            <li class="flex gap-2"><span>&#10145;&#65039;</span> <div><strong>Interruptor Termomagnético:</strong> Dimensionado según la corriente nominal de salida del inversor.</div></li>
                            <li class="flex gap-2"><span>&#10145;&#65039;</span> <div><strong>Interruptor Diferencial:</strong> Inmunizado o Tipo A (no el estándar AC) para evitar saltos intempestivos por la electrónica.</div></li>
                            <li class="flex gap-2"><span>&#10145;&#65039;</span> <div><strong>Interruptor de Corte Visible:</strong> Exigido por algunas distribuidoras cerca del medidor para que los operarios bloqueen la inyección.</div></li>
                        </ul>
                    </div>
                </div>
                
                <div class="mt-6 bg-slate-800 text-white p-5 rounded-xl flex items-start gap-4">
                    <div class="text-3xl">&#127758;</div>
                    <div>
                        <h4 class="font-bold mb-1">Puesta a Tierra (PAT)</h4>
                        <p class="text-sm opacity-90">Es OBLIGATORIO que las estructuras metálicas de los paneles y la carcasa del inversor estén conectadas a tierra. AEA exige equipotencialidad. Si la distancia es grande, se clava una jabalina específica para la estructura solar, vinculada a la principal.</p>
                    </div>
                </div>
            </section>

            <!-- Chapter 6: Materiales -->
            <section id="sec-listado" class="content-section">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 border-b pb-2 mb-4">6. Listado de Materiales (BOM)</h2>
                    <p class="text-slate-600 mb-6">Comparativa general del equipamiento necesario para montar un sistema según la escala del proyecto. Seleccione una pestaña.</p>
                </div>

                <!-- Tabs -->
                <div class="border-b border-slate-200 mb-6 flex gap-4">
                    <button onclick="switchTab('tab-hogar', this)" class="tab-btn active pb-2 border-b-2 border-brand-500 font-bold text-brand-600 px-2">Hogar Residencial (3 kW)</button>
                    <button onclick="switchTab('tab-industria', this)" class="tab-btn pb-2 border-b-2 border-transparent text-slate-500 hover:text-slate-700 px-2">Industria Mediana (50 kW)</button>
                </div>

                <!-- Tab Content: Hogar -->
                <div id="tab-hogar" class="tab-content block">
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                        <ul class="grid md:grid-cols-2 gap-y-4 gap-x-8 text-sm text-slate-700">
                            <li class="flex items-center gap-2"><span class="text-sun-500">&#9632;</span> 6 x Paneles Solares Monocristalinos 500W+</li>
                            <li class="flex items-center gap-2"><span class="text-sun-500">&#9632;</span> 1 x Inversor On-Grid Monofásico 3kW</li>
                            <li class="flex items-center gap-2"><span class="text-sun-500">&#9632;</span> Estructura Coplanar de Aluminio (para techo chapa/teja)</li>
                            <li class="flex items-center gap-2"><span class="text-sun-500">&#9632;</span> 1 x Tablero DC (1 entrada, con seccionador y DPS)</li>
                            <li class="flex items-center gap-2"><span class="text-sun-500">&#9632;</span> 1 x Tablero AC (Termomagnética + Diferencial Tipo A)</li>
                            <li class="flex items-center gap-2"><span class="text-sun-500">&#9632;</span> 30m x Cable Solar 4mm (Rojo/Negro) + Fichas MC4</li>
                            <li class="flex items-center gap-2"><span class="text-sun-500">&#9632;</span> 20m x Cable Sintenax AC 3x4mm</li>
                            <li class="flex items-center gap-2"><span class="text-sun-500">&#9632;</span> Jabalina, cable verde/amarillo desnudo y morcetos.</li>
                        </ul>
                    </div>
                </div>

                <!-- Tab Content: Industria -->
                <div id="tab-industria" class="tab-content hidden">
                    <div class="bg-slate-50 rounded-xl shadow-sm border border-slate-200 p-6">
                        <ul class="grid md:grid-cols-2 gap-y-4 gap-x-8 text-sm text-slate-700">
                            <li class="flex items-center gap-2"><span class="text-slate-500">&#9632;</span> ~90-100 x Paneles Solares Monocristalinos 550W+</li>
                            <li class="flex items-center gap-2"><span class="text-slate-500">&#9632;</span> 1 x Inversor On-Grid Trifásico 50kW (múltiples MPPT)</li>
                            <li class="flex items-center gap-2"><span class="text-slate-500">&#9632;</span> Estructura triangulada aluminio/acero galvanizado</li>
                            <li class="flex items-center gap-2"><span class="text-slate-500">&#9632;</span> Múltiples Tableros DC (String box) con fusibles</li>
                            <li class="flex items-center gap-2"><span class="text-slate-500">&#9632;</span> Tablero AC General robusto con analizador de redes</li>
                            <li class="flex items-center gap-2"><span class="text-slate-500">&#9632;</span> Bobinas de Cable Solar 6mm + Cientos de MC4</li>
                            <li class="flex items-center gap-2"><span class="text-slate-500">&#9632;</span> Bandejas portacables metálicas perforadas</li>
                            <li class="flex items-center gap-2"><span class="text-slate-500">&#9632;</span> Smart Meter (Medidor inteligente para Inyección Cero opcional)</li>
                        </ul>
                    </div>
                </div>
            </section>

            <!-- Chapter 7: Proveedores -->
            <section id="sec-proveedores" class="content-section">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 border-b pb-2 mb-4">7. Proveedores e Importadores en Argentina</h2>
                    <p class="text-slate-600 mb-6">Listado de empresas mayoristas e importadores directos de material certificado (Nota: Datos a modo de ejemplo representativo del mercado, la disponibilidad varía).</p>
                </div>

                <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-4">
                    <!-- Card 1 -->
                    <div class="bg-white border border-slate-200 rounded-lg p-5 hover:shadow-md transition-shadow">
                        <div class="text-xs font-bold text-brand-600 bg-brand-50 inline-block px-2 py-1 rounded mb-2">Importador Mayorista</div>
                        <h3 class="font-bold text-slate-800 text-lg">Helius Latam</h3>
                        <p class="text-xs text-slate-500 mb-3">Marcas: Growatt, Jinko, Fronius</p>
                        <div class="text-sm text-slate-600">
                            <p>&#128231; ventas@helius-ejemplo.com.ar</p>
                            <p>&#128222; +54 11 4444-5555</p>
                            <p>&#128205; Munro, Buenos Aires</p>
                        </div>
                    </div>
                    <!-- Card 2 -->
                    <div class="bg-white border border-slate-200 rounded-lg p-5 hover:shadow-md transition-shadow">
                        <div class="text-xs font-bold text-brand-600 bg-brand-50 inline-block px-2 py-1 rounded mb-2">Distribuidor Oficial</div>
                        <h3 class="font-bold text-slate-800 text-lg">SolarMat SRL</h3>
                        <p class="text-xs text-slate-500 mb-3">Marcas: Huawei, Trina Solar, APSystems</p>
                        <div class="text-sm text-slate-600">
                            <p>&#128231; info@solarmat-ej.com.ar</p>
                            <p>&#128222; +54 341 555-6666</p>
                            <p>&#128205; Rosario / CABA</p>
                        </div>
                    </div>
                    <!-- Card 3 -->
                    <div class="bg-white border border-slate-200 rounded-lg p-5 hover:shadow-md transition-shadow">
                        <div class="text-xs font-bold text-brand-600 bg-brand-50 inline-block px-2 py-1 rounded mb-2">Fabricante Estructuras</div>
                        <h3 class="font-bold text-slate-800 text-lg">Metalúrgica Alusol</h3>
                        <p class="text-xs text-slate-500 mb-3">Perfiles de aluminio coplanar y triángulos</p>
                        <div class="text-sm text-slate-600">
                            <p>&#128231; cotizaciones@alusol-ej.com</p>
                            <p>&#128222; +54 11 3333-1111</p>
                            <p>&#128205; Parque Ind. Burzaco</p>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Chapter 8: Dimensionamiento (Interactive) -->
            <section id="sec-dimensionamiento" class="content-section">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 border-b pb-2 mb-4">8. Guía Práctica: Dimensionamiento Residencial</h2>
                    <p class="text-slate-600 mb-6">El objetivo en Argentina (por el esquema Net Billing) suele ser <strong>cubrir el 100% del consumo energético</strong>, pero no sobredimensionar en exceso, ya que la energía inyectada se paga barato. Usa la calculadora para estimar tu sistema.</p>
                </div>

                <div class="grid lg:grid-cols-5 gap-8">
                    <!-- Calculator Column -->
                    <div class="lg:col-span-2 bg-white p-6 rounded-xl shadow-md border border-slate-200">
                        <h3 class="font-bold text-slate-800 mb-4 flex items-center gap-2"><span class="text-xl">&#128187;</span> Calculadora Rápida</h3>
                        
                        <div class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium text-slate-700 mb-1">Consumo Bimestral (ver Factura)</label>
                                <div class="relative">
                                    <input type="number" id="calc-consumo" value="600" class="w-full border border-slate-300 rounded-lg px-3 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none">
                                    <span class="absolute right-3 top-2 text-slate-400">kWh</span>
                                </div>
                            </div>
                            
                            <div>
                                <label class="block text-sm font-medium text-slate-700 mb-1">Ubicación (Horas Sol Pico - HSP)</label>
                                <select id="calc-hsp" class="w-full border border-slate-300 rounded-lg px-3 py-2 focus:ring-2 focus:ring-brand-500 focus:outline-none">
                                    <option value="4.2">Buenos Aires / Centro (4.2 HSP)</option>
                                    <option value="5.5">Noroeste / Cuyo (5.5 HSP)</option>
                                    <option value="3.5">Patagonia Norte (3.5 HSP)</option>
                                </select>
                            </div>

                            <button onclick="calculateSolar()" class="w-full bg-sun-500 hover:bg-sun-600 text-white font-bold py-2 rounded-lg transition-colors mt-2">
                                Calcular Sistema
                            </button>
                        </div>

                        <div id="calc-results" class="mt-6 p-4 bg-brand-50 rounded-lg border border-brand-100 hidden">
                            <h4 class="text-sm font-bold text-brand-800 mb-2">Resultado Estimado:</h4>
                            <div class="flex justify-between items-center mb-1">
                                <span class="text-sm text-slate-600">Potencia Inversor Sugerida:</span>
                                <span id="res-inversor" class="font-bold text-slate-800">-- kW</span>
                            </div>
                            <div class="flex justify-between items-center mb-1">
                                <span class="text-sm text-slate-600">Cant. Paneles (500W):</span>
                                <span id="res-paneles" class="font-bold text-slate-800">-- unidades</span>
                            </div>
                            <div class="flex justify-between items-center mt-3 pt-2 border-t border-brand-200">
                                <span class="text-xs text-slate-500">Área en techo requerida:</span>
                                <span id="res-area" class="text-xs font-semibold text-slate-700">-- m²</span>
                            </div>
                        </div>
                    </div>

                    <!-- Chart Column -->
                    <div class="lg:col-span-3">
                        <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                            <h3 class="font-bold text-slate-800 mb-2">¿Por qué dimensionar así? (Net Billing)</h3>
                            <p class="text-sm text-slate-500 mb-4">El gráfico muestra un día típico. De día generas más de lo que consumes (inyectas a la red). De noche consumes de la red. Al final del mes, la distribuidora resta el valor económico de lo inyectado a lo consumido.</p>
                            
                            <!-- Mandatory Chart Container formatting -->
                            <div class="chart-container">
                                <canvas id="generationChart"></canvas>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

        </div>
    </main>

    <script>
        // --- Navigation Logic ---
        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.content-section').forEach(sec => {
                sec.classList.remove('active');
            });
            // Show target
            document.getElementById(sectionId).classList.add('active');
            
            // Update Desktop Sidebar active state
            document.querySelectorAll('.nav-item').forEach(btn => {
                btn.classList.remove('active');
                if(btn.getAttribute('onclick').includes(sectionId)) {
                    btn.classList.add('active');
                }
            });

            // Update Mobile Select (if changed via desktop or external link)
            document.getElementById('mobileNav').value = sectionId;

            // Scroll to top
            document.querySelector('main').scrollTop = 0;
        }

        // --- Tabs Logic ---
        function switchTab(tabId, btnElement) {
            // Hide all tabs in the group
            const container = btnElement.parentElement.parentElement;
            container.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.add('hidden');
            });
            // Show target
            document.getElementById(tabId).classList.remove('hidden');
            
            // Update button styles
            container.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('border-brand-500', 'font-bold', 'text-brand-600');
                btn.classList.add('border-transparent', 'text-slate-500');
            });
            btnElement.classList.remove('border-transparent', 'text-slate-500');
            btnElement.classList.add('border-brand-500', 'font-bold', 'text-brand-600');
        }

        // --- Calculator Logic ---
        function calculateSolar() {
            const consumoBimestral = parseFloat(document.getElementById('calc-consumo').value);
            const hsp = parseFloat(document.getElementById('calc-hsp').value);
            
            if(!consumoBimestral || consumoBimestral <= 0) {
                alert("Por favor ingrese un consumo válido.");
                return;
            }

            // Calculations
            const consumoDiario = (consumoBimestral / 60); // kWh/dia
            const perdidasSistema = 0.8; // 20% system losses (heat, dirt, inverter efficiency)
            
            // Potencia Pico necesaria = Consumo Diario / (HSP * Rendimiento)
            const potenciaPicoKW = consumoDiario / (hsp * perdidasSistema);
            
            // Inverter selection (round up to nearest common size: 2, 3, 5 kW)
            let inversorSugerido = 3;
            if(potenciaPicoKW < 2.5) inversorSugerido = 2;
            else if(potenciaPicoKW > 3.5 && potenciaPicoKW <= 5) inversorSugerido = 5;
            else if(potenciaPicoKW > 5) inversorSugerido = Math.ceil(potenciaPicoKW);

            // Panels (Assume 500W / 0.5kW panels)
            const cantPaneles = Math.ceil(potenciaPicoKW / 0.5);
            
            // Area (Assume ~2.2 m2 per 500W panel)
            const areaTecho = cantPaneles * 2.2;

            // Update DOM
            document.getElementById('res-inversor').innerText = inversorSugerido + ' kW';
            document.getElementById('res-paneles').innerText = cantPaneles;
            document.getElementById('res-area').innerText = areaTecho.toFixed(1) + ' m²';
            
            // Show results div
            const resDiv = document.getElementById('calc-results');
            resDiv.classList.remove('hidden');
            resDiv.classList.add('animate-pulse');
            setTimeout(() => resDiv.classList.remove('animate-pulse'), 500);
        }

        // --- Chart.js Logic ---
        document.addEventListener('DOMContentLoaded', function() {
            const ctx = document.getElementById('generationChart').getContext('2d');
            
            // Data for a typical day
            const labels = ['00', '02', '04', '06', '08', '10', '12', '14', '16', '18', '20', '22'];
            const consumoHogar = [0.5, 0.4, 0.4, 0.8, 1.2, 1.0, 1.5, 1.2, 1.0, 1.8, 2.5, 1.5]; // kWh
            const generacionSolar = [0, 0, 0, 0.2, 1.5, 3.0, 4.0, 3.5, 2.0, 0.5, 0, 0]; // kWh (Bell curve)

            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [
                        {
                            label: 'Generación Solar',
                            data: generacionSolar,
                            borderColor: '#f59e0b', // sun-500
                            backgroundColor: 'rgba(245, 158, 11, 0.2)',
                            borderWidth: 2,
                            fill: true,
                            tension: 0.4
                        },
                        {
                            label: 'Consumo Hogar',
                            data: consumoHogar,
                            borderColor: '#334155', // slate-700
                            backgroundColor: 'transparent',
                            borderWidth: 2,
                            borderDash: [5, 5],
                            fill: false,
                            tension: 0.4
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false, // CRITICAL for chart-container sizing
                    interaction: {
                        mode: 'index',
                        intersect: false,
                    },
                    plugins: {
                        legend: {
                            position: 'top',
                            labels: {
                                font: { family: 'Inter' }
                            }
                        },
                        tooltip: {
                            backgroundColor: 'rgba(15, 23, 42, 0.9)', // slate-900
                            titleFont: { family: 'Inter' },
                            bodyFont: { family: 'Inter' },
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed.y !== null) {
                                        label += context.parsed.y + ' kWh';
                                    }
                                    // Wrap logic concept (though short labels here)
                                    if (label.length > 25) {
                                         label = label.match(/.{1,25}(\s|$)/g);
                                    }
                                    return label;
                                }
                            }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Potencia (kW)',
                                font: { family: 'Inter' }
                            }
                        },
                        x: {
                            title: {
                                display: true,
                                text: 'Hora del día',
                                font: { family: 'Inter' }
                            }
                        }
                    }
                }
            });
        });
    </script>
</body>
</html>
