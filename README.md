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
        }
        .bg-primary { background-color: var(--color-primary); }
        .text-primary { color: var(--color-primary); }
        .bg-secondary { background-color: var(--color-secondary); }
        .bg-accent { background-color: var(--color-accent); }
        
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
            margin: 10% auto;
            max-width: 400px;
        }
    </style>
</head>
<body class="antialiased">

    <!-- WhatsApp Flotante -->
    <a id="whatsapp-link" href="#" target="_blank" class="whatsapp-float bg-green-500 text-white p-3 rounded-full flex items-center justify-center w-14 h-14 hover:bg-green-600 transition duration-300">
        <!-- Icono de WhatsApp (SVG) -->
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><path d="M.057 24l1.687-6.109c-1.045-1.849-1.637-3.999-1.637-6.205 0-6.904 5.602-12.506 12.506-12.506 3.398 0 6.51 1.341 8.887 3.718 2.378 2.376 3.719 5.487 3.719 8.885 0 6.905-5.602 12.507-12.506 12.507-2.072 0-4.14-.509-5.938-1.472l-6.206 1.687zm6.75-5.32c1.235.808 2.895 1.4 4.502 1.4 5.378 0 9.743-4.364 9.743-9.743s-4.365-9.744-9.743-9.744-9.743 4.365-9.743 9.744c0 1.956.574 3.896 1.761 5.32l.271.371-.853 3.12 3.298-.823.336.195c1.442.827 3.09 1.258 4.79 1.258zm-.62-2.583l-1.028-.668-.002-.002c-1.11-.722-1.748-1.89-1.748-3.149 0-2.695 2.193-4.887 4.886-4.887 1.35 0 2.593.558 3.488 1.453.895.895 1.453 2.138 1.453 3.488 0 2.693-2.193 4.885-4.885 4.885-.733 0-1.464-.176-2.112-.519l-1.178-.588z"/></svg>
    </a>

    <!-- Modal de Formulario de Pago (Oculto por defecto) -->
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
                <button type="submit" class="w-full bg-accent text-primary font-bold py-3 px-4 rounded-lg shadow-lg hover:bg-yellow-600 transition duration-300">
                    Ir a Pago Seguro
                </button>
            </form>
            <button onclick="closeModal()" class="mt-4 w-full text-sm text-gray-500 hover:text-gray-700">Cancelar y Volver</button>
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
            </p>
            <p class="mt-2 text-sm text-primary font-semibold">- Albert Schweitzer (Filosofía de Mentalidad) -</p>
        </div>
    </section>

    <!-- Sección de Cursos -->
    <section class="py-16 md:py-24">
        <div class="max-w-6xl mx-auto px-6">
            <h2 class="text-3xl md:text-4xl font-bold text-center mb-12 text-gray-800">
                4. Explora Nuestros 3 Niveles de Aprendizaje
            </h2>

            <div class="grid md:grid-cols-3 gap-8">
                
                <!-- Tarjeta 1: Curso Gratuito (Lead Magnet) - AHORA VIDEO DIRECTO -->
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border-t-8 border-yellow-500 flex flex-col justify-between transform hover:scale-[1.02] transition duration-300">
                    <div>
                        <span class="text-4xl mb-4 block">📘</span>
                        <h3 class="text-2xl font-bold mb-3 text-gray-900">Curso Gratuito: Introducción a la Importación</h3>
                        <!-- Duración actualizada a 50 minutos -->
                        <p class="text-sm text-gray-600 mb-4">Duración: <b>50 minutos</b> (Clase en Video)</p>
                        <p class="text-gray-700 mb-6">
                            <b>Objetivo:</b> Entender cómo funciona el sistema de importación y venta online.
                        </p>
                        <p class="text-sm italic text-gray-500 mt-auto">
                            Ideal para quienes quieren descubrir si el comercio digital es para ellos.
                        </p>
                    </div>
                    <!-- REDIRECCIÓN AL VIDEO DE YOUTUBE (reemplazar YOUR_UNLISTED_VIDEO_ID) -->
                    <a id="cta-free-course" href="https://www.youtube.com/watch?v=YOUR_UNLISTED_VIDEO_ID" target="_blank" class="mt-6 w-full text-center bg-gray-200 text-gray-800 font-bold py-3 rounded-lg hover:bg-gray-300 transition duration-300">
                        Ver Clase GRATUITA (50 min)
                    </a>
                </div>

                <!-- Tarjeta 2: Curso Básico (Inversión Inicial) - TÍTULO COMPLETO -->
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-lg border-t-8 border-blue-500 flex flex-col justify-between transform hover:scale-[1.02] transition duration-300">
                    <div>
                        <span class="text-4xl mb-4 block">📗</span>
                        <h3 class="text-2xl font-bold mb-3 text-gray-900">📗 Curso Básico: Bases de un Negocio Online</h3>
                        <p class="text-sm text-gray-600 mb-4">Duración: <b>2 horas</b> | <b>Precio: Gs. 220.000</b></p>
                        <p class="text-gray-700 mb-6">
                            <b>Objetivo:</b> Mostrar los fundamentos reales de un negocio online rentable.
                        </p>
                        <p class="text-sm italic text-gray-500 mt-auto">
                            Perfecto para quienes quieren empezar con su primer producto y venta real.
                        </p>
                    </div>
                    <button onclick="openModal('Básico')" class="mt-6 w-full bg-blue-500 text-white font-bold py-3 rounded-lg hover:bg-blue-600 transition duration-300">
                        Comprar Curso Básico
                    </button>
                </div>

                <!-- Tarjeta 3: Curso Avanzado (PRIORIDAD DE VENTA) - TÍTULO COMPLETO Y LANZAMIENTO ESPECIFICADO -->
                <div class="bg-white p-6 md:p-8 rounded-xl shadow-2xl border-t-8 border-primary flex flex-col justify-between transform hover:scale-[1.02] transition duration-300 relative">
                     <span class="absolute top-0 right-0 bg-red-500 text-white text-xs font-bold px-3 py-1 rounded-bl-lg">MÁS VENDIDO</span>
                    <div>
                        <span class="text-4xl mb-4 block">📕</span>
                        <h3 class="text-2xl font-bold mb-3 text-primary">📕 Curso Avanzado: El Método Fer Verón</h3>
                        <p class="text-sm font-black text-red-600 mb-2">¡Lanzamiento: 16 de Noviembre!</p>
                        <p class="text-sm text-gray-600 mb-4">Duración: <b>5 Módulos (5-6h)</b> | <b>Precio: Gs. 550.000</b></p>
                        <p class="text-gray-700 mb-6">
                            <b>Objetivo:</b> Construir, escalar y automatizar un negocio digital completo.
                        </p>
                        <ul class="list-none space-y-2 text-sm text-gray-700 mb-6">
                            <li class="flex items-center"><span class="text-green-500 mr-2">✅</span> Grupo de alumnos exclusivo</li>
                            <li class="flex items-center"><span class="text-green-500 mr-2">✅</span> Planilla financiera profesional</li>
                            <li class="flex items-center"><span class="text-green-500 mr-2">✅</span> Plantillas de anuncios probadas</li>
                            <li class="flex items-center"><span class="text-green-500 mr-2">✅</span> Módulo de IA aplicada a negocios</li>
                        </ul>
                        <!-- Texto actualizado para el descuento/continuación -->
                        <p class="text-sm font-semibold text-gray-500">
                            ¿Y hiciste el curso básico? Continúa con el avanzado por solo <b>Gs 330.000</b>
                        </p>
                    </div>
                    <button onclick="openModal('Avanzado')" class="mt-6 w-full bg-accent text-primary font-black py-4 rounded-lg shadow-xl hover:bg-yellow-600 transition duration-300 text-lg">
                        ¡Quiero el Método Completo!
                    </button>
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
        const courses = {
            'Básico': { price: 'Gs. 220.000', whatsapp: 'Hola! Estoy interesado en comprar el Curso Básico: Bases de un Negocio Online (Gs. 220.000). ¿Podrías indicarme los medios de pago?' },
            'Avanzado': { price: 'Gs. 550.000', whatsapp: 'Hola! Estoy listo para comprar el Curso Avanzado: El Método Fer Verón (Gs. 550.000). ¿Podrías indicarme los medios de pago?' },
        };

        let selectedCourse = '';
        
        // 1. Configurar el botón flotante de WhatsApp
        const waLink = document.getElementById('whatsapp-link');
        const defaultMessage = "Hola! Me gustaría ser parte de tu curso ¿Podrías indicarme los medios de pago?";
        const encodedDefaultMessage = encodeURIComponent(defaultMessage);
        
        // Asumiendo un número de teléfono de ejemplo en Paraguay (+595)
        const phoneNumber = '595991XXXXXX'; // Reemplaza XXXXXX con tu número real
        waLink.href = `https://wa.me/${phoneNumber}?text=${encodedDefaultMessage}`;

        // 2. Funcionalidad del Modal/Formulario
        function openModal(courseKey) {
            selectedCourse = courseKey;
            document.getElementById('paymentModal').style.display = 'block';
            
            const modalTitle = document.querySelector('#paymentModal h3');
            const modalPrice = courses[courseKey].price;
            
            modalTitle.textContent = `¡Estás comprando el curso ${courseKey} (${modalPrice})!`;
            
            // Reemplazamos la funcionalidad del botón de ir a pago por la simulación
            const submitButton = document.querySelector('#paymentForm button[type="submit"]');
            submitButton.textContent = `Ir a Pago del Curso ${courseKey} (${modalPrice})`;
        }

        function closeModal() {
            document.getElementById('paymentModal').style.display = 'none';
        }

        // Simular el proceso de pago y seguimiento del lead
        document.getElementById('paymentForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const phone = document.getElementById('phone').value;
            
            // Aquí simularías:
            // 1. Envío de datos a tu CRM/Base de Datos (el lead)
            // 2. Redirección real a la pasarela de pago (ej. PayPal, Mercado Pago, etc.)

            console.log("--- LEAD CAPTURED ---");
            console.log(`Curso: ${selectedCourse}`);
            console.log(`Nombre: ${name}`);
            console.log(`Email: ${email}`);
            console.log(`Teléfono: ${phone}`);
            
            // Uso de un div de mensaje en lugar de alert()
            const modalContent = document.getElementById('paymentModal').querySelector('.modal-content');
            modalContent.innerHTML = `
                <h3 class="text-2xl font-bold mb-4 text-green-600">¡Datos Capturados!</h3>
                <p class="mb-6 text-gray-700">Gracias, <b>${name}</b>.</p>
                <p class="mb-6 text-gray-700">Te estamos redirigiendo a la pasarela de pago seguro para finalizar la compra del <b>Curso ${selectedCourse}</b>.</p>
                <button onclick="closeModal()" class="w-full bg-primary text-white font-bold py-3 px-4 rounded-lg shadow-lg hover:bg-blue-800 transition duration-300">
                    Cerrar
                </button>
            `;

            // En un entorno real, la siguiente línea sería:
            // window.location.href = "URL_DE_TU_PASARELA_DE_PAGO";
        });
        
        // Cerrar el modal al hacer clic fuera de él
        window.onclick = function(event) {
            const modal = document.getElementById('paymentModal');
            if (event.target == modal) {
                closeModal();
            }
        }
    </script>
</body>
</html>
