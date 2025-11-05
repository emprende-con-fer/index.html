<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>El Método Fer Verón - Negocios Digitales</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap" rel="stylesheet">
    <style>
        /* Definición de la paleta sobria */
        :root {
            --color-primary: #1e3a8a; /* Azul oscuro sobrio */
            --color-secondary: #bfdbfe; /* Azul celeste tenue */
            --color-accent: #f59e0b; /* Naranja/dorado para el CTA */
            --color-premium: #5b21b6; /* Morado para Asesoría Premium */
        }
        .bg-primary { background-color: var(--color-primary); }
        .text-primary { color: var(--color-primary); }
        .bg-secondary { background-color: var(--color-secondary); }
        .bg-accent { background-color: var(--color-accent); }
        .bg-premium { background-color: var(--color-premium); }
        .text-premium { color: var(--color-premium); }
        
        body { font-family: 'Inter', sans-serif; background-color: #f9fafb; }

        /* Estilos para el botón de WhatsApp flotante */
        .whatsapp-float {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 1000;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s ease;
        }
        .whatsapp-float:hover {
            transform: scale(1.05);
        }

        /* Estilo para el modal/formulario */
        .modal {
            display: none;
            position: fixed;
            z-index: 2000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.7);
        }
        .modal-content {
            margin: 5% auto; /* Margen reducido para formularios más largos */
            max-width: 500px; /* Ancho aumentado para el formulario de asesoría */
        }
    </style>
</head>
<body class="antialiased">

    <!-- WhatsApp Flotante (Con mensaje para medios de pago generales) -->
    <a id="whatsapp-link" href="#" target="_blank" class="whatsapp-float bg-green-500 text-white p-3 rounded-full flex items-center justify-center w-14 h-14 hover:bg-green-600 transition duration-300">
        <!-- Icono de WhatsApp (SVG) -->
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><path d="M.057 24l1.687-6.109c-1.045-1.849-1.637-3.999-1.637-6.205 0-6.904 5.602-12.506 12.506-12.506 3.398 0 6.51 1.341 8.887 3.718 2.378 2.376 3.719 5.487 3.719 8.885 0 6.905-5.602 12.507-12.506 12.507-2.072 0-4.14-.509-5.938-1.472l-6.206 1.687zm6.75-5.32c1.235.808 2.895 1.4 4.502 1.4 5.378 0 9.743-4.364 9.743-9.743s-4.365-9.744-9.743-9.744-9.743 4.365-9.743 9.744c0 1.956.574 3.896 1.761 5.32l.271.371-.853 3.12 3.298-.823.336.195c1.442.827 3.09 1.258 4.79 1.258zm-.62-2.583l-1.028-.668-.002-.002c-1.11-.722-1.748-1.89-1.748-3.149 0-2.695 2.193-4.887 4.886-4.887 1.35 0 2.593.558 3.488 1.453.895.895 1.453 2.138 1.453 3.488 0 2.693-2.193 4.885-4.885 4.885-.733 0-1.464-.176-2.112-.519l-1.178-.588z"/></svg>
    </a>

    <!-- Modal de Formulario de Pago (Oculto por defecto, usado para Avanzado) -->
    <div id="paymentModal" class="modal">
        <div class="modal-content bg-white p-8 rounded-xl shadow-2xl">
            <h3 class="text-2xl font-bold mb-4 text-primary">Paso Final: Captura de Datos</h3>
            <p class="mb-6 text-gray-600">Completa tus datos para recibir la confirmación de pago y acceso a la comunidad privada.</p>
            <form id="paymentForm">
                <div class="mb-4">
                    <label for="name" class="block text-sm font-medium text-gray-700">Nombre Completo</label>
                    <input type="text" id="name" name="name" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-primary focus:ring-primary">
                </div>
                <div class="mb-4">
                    <label for="email" class="block text-sm font-medium text-gray-700">Correo Electrónico</label>
                    <input type="email" id="email" name="email" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-primary focus:ring-primary">
                </div>
                <div class="mb-6">
                    <label for="phone" class="block text-sm font-medium text-gray-700">Número de Teléfono</label>
                    <input type="tel" id="phone" name="phone" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-primary focus:ring-primary">
                </div>
                <button type="submit" class="w-full bg-accent text-primary font-black py-3 px-4 rounded-lg shadow-lg hover:bg-yellow-600 transition duration-300">
                    Ir a Pago Seguro
                </button>
            </form>
            <button onclick="closeModal('paymentModal')" class="mt-4 w-full text-sm text-gray-500 hover:text-gray-700">Cancelar y Volver</button>
        </div>
    </div>

    <!-- *** NUEVO MODAL: Formulario de Calificación para Asesoría *** -->
    <div id="asesoriaModal" class="modal">
        <div class="modal-content bg-white p-8 rounded-xl shadow-2xl">
            <h3 class="text-2xl font-bold mb-4 text-premium">Solicitud de Asesoría 1:1</h3>
            <p class="mb-6 text-gray-600">Completa este formulario de pre-calificación. Nos ayudará a entender si podemos ayudarte a escalar tu negocio.</p>
            
            <form id="asesoriaForm">
                <!-- Datos de Contacto -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                    <div>
                        <label for="asesoria_name" class="block text-sm font-medium text-gray-700">Nombre</label>
                        <input type="text" id="asesoria_name" name="asesoria_name" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-premium focus:ring-premium">
                    </div>
                    <div>
                        <label for="asesoria_phone" class="block text-sm font-medium text-gray-700">N° de Teléfono</LAbel>
                        <input type="tel" id="asesoria_phone" name="asesoria_phone" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-premium focus:ring-premium">
                    </div>
                </div>
                <div class="mb-4">
                    <label for="asesoria_email" class="block text-sm font-medium text-gray-700">Correo Electrónico</label>
                    <input type="email" id="asesoria_email" name="asesoria_email" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-premium focus:ring-premium">
                </div>
                <hr class="my-6">
                
                <!-- Preguntas de Calificación -->
                <div class="mb-4">
                    <label for="nicho_negocio" class="block text-sm font-medium text-gray-700">1. Nicho del negocio (qué vendes: servicios, productos, qué tipo de productos)</label>
                    <textarea id="nicho_negocio" name="nicho_negocio" rows="2" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-premium focus:ring-premium"></textarea>
                </div>
                <div class="mb-4">
                    <label for="facturacion_mensual" class="block text-sm font-medium text-gray-700">2. Facturación mensual actual (en Gs.)</label>
                    <input type="text" id="facturacion_mensual" name="facturacion_mensual" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-premium focus:ring-premium" placeholder="Ej: 3.000.000">
                </div>
                <div class="mb-4">
                    <label for="invierte_anuncios" class="block text-sm font-medium text-gray-700">3. ¿Inviertes en anuncios?</label>
                    <select id="invierte_anuncios" name="invierte_anuncios" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-premium focus:ring-premium">
                        <option>Sí</option>
                        <option>No</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label for="inversion_mensual" class="block text-sm font-medium text-gray-700">4. ¿Cuánto inviertes mensualmente? (en Gs.)</label>
                    <input type="text" id="inversion_mensual" name="inversion_mensual" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-premium focus:ring-premium" placeholder="Ej: 500.000 o '0' si no inviertes">
                </div>
                <div class="mb-6">
                    <label for="agencia_marketing" class="block text-sm font-medium text-gray-700">5. ¿Trabajas con una agencia de marketing o lo haces solo?</label>
                    <select id="agencia_marketing" name="agencia_marketing" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 border focus:border-premium focus:ring-premium">
                        <option>Lo hago solo</option>
                        <option>Trabajo con una agencia</option>
                        <option>Trabajo con un freelance</option>
                    </select>
                </div>

                <button type="submit" class="w-full bg-premium text-white font-black py-3 px-4 rounded-lg shadow-lg hover:bg-purple-800 transition duration-300">
                    Enviar Solicitud
                </button>
            </form>
            <button onclick="closeModal('asesoriaModal')" class="mt-4 w-full text-sm text-gray-500 hover:text-gray-700">Cancelar</button>
        </div>
    </div>


    <!-- Sección Principal (Hero) -->
    <header class="bg-primary text-white py-16 md:py-24">
        <div class="max-w-4xl mx-auto px-6 text-center">
            <h1 class="text-4xl md:text-6xl font-extrabold mb-4">
                Importa y Vende Online: El Método que Elimina los Errores de Novato.
            </h1>
            <p class="text-xl md:text-2xl font-light mb-8 opacity-90">
                Aprende el Método exacto para <b>Importar y Vender Online</b> sin invertir grandes sumas y desde Paraguay.
            </p>
            <!-- CTA Principal que activa el modal de pago -->
            <button onclick="openModal('Avanzado')" class="bg-accent text-primary font-black py-4 px-10 rounded-xl text-lg md:text-xl shadow-lg hover:shadow-xl transform hover:scale-105 transition duration-300">
                Descubre el Método Fer Verón 🚀
            </button>
        </div>
    </header>

    <!-- Frase Motivacional 1 -->
    <section class="py-12 bg-gray-50">
        <div class="max-w-3xl mx-auto px-6 text-center">
            <p class="text-xl italic text-gray-700">
                "El éxito no es la clave de la felicidad. La felicidad es la clave del éxito. Si amas lo que haces, tendrás éxito."
            </A>
            <p class="mt-2 text-sm text-primary font-semibold">- Albert Schweitzer (Filosofía de Mentalidad) -</p>
        </div>
    </section>

    <!-- Sección de Cursos Grabados (Grid de 3 columnas en desktop) -->
    <section class="py-16 md:py-24">
        <div class="max-w-7xl mx-auto px-6">
            <h2 class="text-3xl md:text-4xl font-bold text-center mb-12 text-gray-800">
                1. Cursos Digitales Grabados
            </h2>

            <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
                
                <!-- Tarjeta 1: Curso Gratuito (Lead Magnet) -->
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border-t-8 border-yellow-500 flex flex-col justify-between transform hover:scale-[1.02] transition duration-300">
                    <div>
                        <span class="text-4xl mb-4 block">📘</span>
                        <h3 class="text-xl font-bold mb-3 text-gray-900">Curso Gratuito: Introducción a la Importación</h3>
                        <!-- Duración: 30 minutos -->
                        <p class="text-sm text-gray-600 mb-4">Duración: <b>30 minutos</b> (Clase en Video)</p>
                        <p class="text-gray-700 mb-6 text-sm">
                            <b>Objetivo:</b> Entender cómo funciona el sistema de importación y venta online.
                        </p>
                        <p class="text-xs italic text-gray-500 mt-auto">
                            Ideal para quienes quieren descubrir si el comercio digital es para ellos.
                        </p>
                    </div>
                    <!-- *** CAMBIO: Enlace de YouTube actualizado *** -->
                    <a id="cta-free-course" href="https://youtu.be/jvDEOVCFwzY?si=2WuN-AxnXMghlDGR" target="_blank" class="mt-6 w-full text-center bg-gray-200 text-gray-800 font-bold py-3 rounded-lg hover:bg-gray-300 transition duration-300 text-sm">
                        <!-- Botón actualizado a 30 min -->
                        Ver Clase GRATUITA (30 min)
                    </a>
                </div>

                <!-- Tarjeta 2: Curso Básico (Inversión Inicial) - WHATSAPP PERSONALIZADO -->
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border-t-8 border-blue-500 flex flex-col justify-between transform hover:scale-[1.02] transition duration-300">
                    <div>
                        <span class="text-4xl mb-4 block">📗</span>
                        <h3 class="text-xl font-bold mb-3 text-gray-900">📗 Curso Básico: Bases de un Negocio Online</h3>
                        <!-- Duración eliminada -->
                        <p class="text-sm text-gray-600 mb-4"><b>Precio: Gs. 220.000</b></p>
                        <p class="text-gray-700 mb-6 text-sm">
                            <b>Objetivo:</b> Mostrar los fundamentos reales de un negocio online rentable.
                        </p>
                        <p class="text-xs italic text-gray-500 mt-auto">
                            Perfecto para quienes quieren empezar con su primer producto y venta real.
                        </p>
                    </div>
                    <!-- CTA que redirige a WhatsApp con mensaje específico de Básico -->
                    <a id="cta-basic-whatsapp" href="#" target="_blank" class="mt-6 w-full text-center bg-blue-500 text-white font-bold py-3 rounded-lg hover:bg-blue-600 transition duration-300 text-sm">
                        Comprar Curso Básico
                    </a>
                </div>

                <!-- Tarjeta 3: Curso Avanzado (PRIORIDAD DE VENTA) - LANZAMIENTO Y DESCUENTO -->
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-2xl border-t-8 border-primary flex flex-col justify-between transform hover:scale-[1.02] transition duration-300 relative">
                     <span class="absolute top-0 right-0 bg-red-500 text-white text-xs font-bold px-3 py-1 rounded-bl-lg">MÁS VENDIDO</span>
                    <div>
                        <span class="text-4xl mb-4 block">📕</span>
                        <h3 class="text-xl font-bold mb-3 text-primary">📕 Curso Avanzado: El Método Fer Verón</h3>
                        <p class="text-sm font-black text-red-600 mb-2">¡Lanzamiento: 16 de Noviembre!</p>
                        <p class="text-sm text-gray-600 mb-4">Duración: <b>5 Módulos (5-6h)</b> | <b>Precio: Gs. 550.000</b></p>
                        <p class="text-gray-700 mb-6 text-sm">
                            <b>Objetivo:</b> Construir, escalar y automatizar un negocio digital completo.
                        </p>
                        <ul class="list-none space-y-2 text-sm text-gray-700 mb-4">
                            <li class="flex items-center"><span class="text-green-500 mr-2">✅</span> Grupo de alumnos exclusivo</li>
                            <li class="flex items-center"><span class="text-green-500 mr-2">✅</span> Planilla financiera profesional</li>
                            <!-- ... otros bonuses ... -->
                        </ul>
                        <!-- Texto actualizado para el descuento/continuación -->
                        <p class="text-xs font-semibold text-gray-500">
                            ¿Y hiciste el curso básico? Continúa con el avanzado por solo <b>Gs 330.000</b>
                        </p>
                    </div>
                    <button onclick="openModal('Avanzado')" class="mt-6 w-full bg-accent text-primary font-black py-3 rounded-lg shadow-xl hover:bg-yellow-600 transition duration-300 text-sm">
                        ¡Quiero el Método Completo!
                    </button>
                </div>
            </div>
        </div>
    </section>

    <!-- Sección 2: Asesoría Personalizada 1:1 (Destacada y Separada) -->
    <section class="py-12 md:py-20 bg-gray-100 border-t-4 border-gray-200">
        <div class="max-w-4xl mx-auto px-6">
            <h2 class="text-3xl md:text-4xl font-bold text-center mb-8 text-premium">
                2. Asesoría Personalizada 1:1: El Salto de Emprendedores Serios
            </h2>
        
            <!-- Tarjeta 4: Asesoría Personalizada 1:1 (Servicio Premium) -->
            <div class="bg-white p-6 md:p-8 rounded-xl shadow-2xl border-t-8 border-premium flex flex-col justify-between transform hover:scale-[1.01] transition duration-300">
                <div class="flex flex-col md:flex-row gap-6">
                    <div class="md:w-3/4">
                        <div class="flex items-center mb-4">
                            <span class="text-4xl mr-4 text-premium">💎</span>
                            <h3 class="text-2xl font-bold text-premium">Asesoría Personalizada 1:1 (Premium)</h3>
                        </div>
                        
                        <p class="text-gray-700 mb-4 text-sm">
                            <b>Objetivo:</b> Abordar tus desafíos específicos de crecimiento, optimización y escalabilidad. La sesión es **Presencial u Online** dependiendo de la persona.
                        </p>
                        <!-- Eliminados los iconos '🎯' (asteriscos) -->
                        <ul class="list-none space-y-2 text-sm text-gray-700 mb-6 p-0 ml-6">
                            <li class="pl-2">Ajustamos los conocimientos de las clases acorde a <b>tu necesidad</b> y nicho.</li>
                            <li class="pl-2">Oportunidad de realizar preguntas personalizadas de tu negocio (marca personal o compra/venta).</li>
                        </ul>
                        <p class="text-xs italic text-gray-500 mt-auto">
                            Dirigido exclusivamente a emprendedores que ya están facturando <b>Gs. 3.000.000+</b> para garantizar el impacto.
                        </p>
                    </div>
                    <div class="md:w-1/4 flex flex-col justify-center items-center">
                        <p class="text-sm font-black text-gray-600 mb-4 text-center">
                            Próximo Nivel:
                        </p>
                        <!-- Botón ahora abre el modal 'asesoriaModal' y texto actualizado -->
                        <button id="cta-asesoria-form" onclick="openModal('Asesoria')" class="w-full text-center bg-premium text-white font-bold py-3 px-4 rounded-lg hover:bg-purple-700 transition duration-300 text-sm shadow-md">
                            Solicitar Cotización
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Sección de Pie de Página (Footer) -->
    <footer class="bg-gray-800 text-white py-8">
        <div class="max-w-6xl mx-auto px-6 text-center text-sm">
            <p>&copy; 2025 El Método Fer Verón. Todos los derechos reservados.</p>
            <p class="mt-2 text-gray-400">Política de Privacidad | Términos y Condiciones</p>
        </div>
    </footer>

    <script>
        // Número de WhatsApp para contacto/asesoría
        const CONTACT_PHONE_NUMBER = '595992527736'; 
        
        const courses = {
            'Básico': { 
                price: 'Gs. 220.000', 
                whatsappMessage: "Hola! me gustaría adquirir el curso básico, ¿podrías pasarme tus medios de pago?" 
            },
            'Avanzado': { 
                price: 'Gs. 550.000'
            },
            'Asesoría': {
                // Mensaje base para el formulario de calificación
                whatsappMessage: "Hola, me gustaría solicitar una la asesoría personalizada ¿cómo puedo saber si es para mi?"
            }
        };

        let selectedCourse = '';
        
        // --- FUNCIONES DE WHATSAPP ---

        // Función para generar el enlace de WhatsApp
        function getWhatsAppLink(message) {
            const encodedMessage = encodeURIComponent(message);
            return `https://wa.me/${CONTACT_PHONE_NUMBER}?text=${encodedMessage}`;
        }
        
        // 1. Configurar el botón flotante (Mensaje general de medios de pago)
        const waLink = document.getElementById('whatsapp-link');
        const defaultMessage = "Hola! Me gustaría ser parte de tu curso ¿Podrías indicarme los medios de pago?";
        waLink.href = getWhatsAppLink(defaultMessage);

        // 2. Configurar el botón del Curso Básico (Mensaje de compra)
        document.getElementById('cta-basic-whatsapp').href = getWhatsAppLink(courses['Básico'].whatsappMessage);
        
        // 3. El botón de Asesoría ya NO va a WhatsApp directamente, abre un modal.

        // --- FUNCIONES DE MODAL Y FORMULARIO (para Avanzado y Asesoría) ---

        function openModal(courseKey) {
            selectedCourse = courseKey;
            
            if (courseKey === 'Avanzado') {
                document.getElementById('paymentModal').style.display = 'block';
                const modalTitle = document.querySelector('#paymentModal h3');
                const modalPrice = courses[courseKey].price;
                modalTitle.textContent = `¡Estás comprando el curso ${courseKey} (${modalPrice})!`;
                
                const submitButton = document.querySelector('#paymentForm button[type="submit"]');
                submitButton.textContent = `Ir a Pago del Curso ${courseKey} (${modalPrice})`;
            
            } else if (courseKey === 'Asesoria') {
                document.getElementById('asesoriaModal').style.display = 'block';
            }
        }

        function closeModal(modalId) {
            document.getElementById(modalId).style.display = 'none';
        }

        // --- MANEJADOR FORMULARIO CURSO AVANZADO ---
        document.getElementById('paymentForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const name = document.getElementById('name').value;
            // ... (resto de la lógica de captura de lead y redirección de pago)
            
            console.log("--- LEAD CAPTURED (AVANZADO) ---");
            console.log(`Curso: ${selectedCourse}`);
            console.log(`Nombre: ${name}`);
            
            const modalContent = document.getElementById('paymentModal').querySelector('.modal-content');
            modalContent.innerHTML = `
                <h3 class="text-2xl font-bold mb-4 text-green-600">¡Datos Capturados!</h3>
                <p class="mb-6 text-gray-700">Gracias, <b>${name}</b>. Te estamos redirigiendo a la pasarela de pago seguro para finalizar la compra del <b>Curso ${selectedCourse}</b>.</p>
                <button onclick="closeModal('paymentModal')" class="w-full bg-primary text-white font-bold py-3 px-4 rounded-lg shadow-lg hover:bg-blue-800 transition duration-300">
                    Cerrar
                </button>
            `;

            // En un entorno real, la siguiente línea sería:
            // window.location.href = "URL_DE_TU_PASARELA_DE_PAGO";
        });

        // --- *** NUEVO MANEJADOR FORMULARIO ASESORÍA *** ---
        document.getElementById('asesoriaForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Recabar todos los datos del formulario de asesoría
            const name = document.getElementById('asesoria_name').value;
            const phone = document.getElementById('asesoria_phone').value;
            const email = document.getElementById('asesoria_email').value;
            const nicho = document.getElementById('nicho_negocio').value;
            const facturacion = document.getElementById('facturacion_mensual').value;
            const invierte = document.getElementById('invierte_anuncios').value;
            const inversion = document.getElementById('inversion_mensual').value;
            const agencia = document.getElementById('agencia_marketing').value;

            // Construir el mensaje de WhatsApp detallado
            let message = `Hola Fer, me gustaría solicitar la asesoría personalizada.\n\n--- Formulario de Calificación ---\n\n`;
            message += `*Nombre:* ${name}\n`;
            message += `*Teléfono:* ${phone}\n`;
            message += `*Email:* ${email}\n\n`;
            message += `*1. Nicho:* ${nicho}\n`;
            message += `*2. Facturación Mensual:* ${facturacion} Gs.\n`;
            message += `*3. Invierte en Anuncios:* ${invierte}\n`;
            message += `*4. Inversión Mensual:* ${inversion} Gs.\n`;
            message += `*5. Gestión de Marketing:* ${agencia}\n\n`;
            message += `¿Soy un buen candidato para la asesoría?`;

            // Generar el enlace de WhatsApp y abrirlo en una nueva pestaña
            const whatsappUrl = getWhatsAppLink(message);
            window.open(whatsappUrl, '_blank');
            
            // Mostrar mensaje de éxito en el modal
            const modalContent = document.getElementById('asesoriaModal').querySelector('.modal-content');
            modalContent.innerHTML = `
                <h3 class="text-2xl font-bold mb-4 text-green-600">¡Solicitud Enviada!</h3>
                <p class="mb-6 text-gray-700">Gracias, <b>${name}</b>. Se ha generado tu mensaje de WhatsApp.</p>
                <p class="mb-4 text-sm text-gray-600">Por favor, presiona 'Enviar' en la ventana de WhatsApp que se acaba de abrir para que podamos revisar tu solicitud.</p>
                <button onclick="closeModal('asesoriaModal')" class="w-full bg-premium text-white font-bold py-3 px-4 rounded-lg shadow-lg hover:bg-purple-800 transition duration-300">
                    Cerrar
                </button>
            `;
        });
        
        // Cerrar los modales al hacer clic fuera de ellos
        window.onclick = function(event) {
            const paymentModal = document.getElementById('paymentModal');
            const asesoriaModal = document.getElementById('asesoriaModal');
            if (event.target == paymentModal) {
                closeModal('paymentModal');
            }
            if (event.target == asesoriaModal) {
                closeModal('asesoriaModal');
            }
        }
    </script>
</body>
</html>
