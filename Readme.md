# 1. Настройка со стороны конфигурации cubeIDE:

1. Создаём **новый проект.**
2. В **RCC High Speed Clock (HSE)** ставим в **Crystal/Ceramic Resonator**. (**Low speed Clock (LSE) Не обязательно** включать и ставить в BYPASS Clock Source):

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/033b1711-d04e-41bb-b737-b9320b2c0dc7/Untitled.png)

1. Переход в **SYS**, debug ввести в режим **Serial Wire**, затем в **Time base Source** выбрать какой-нибудь **TIM**:

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/bc022464-7400-4feb-80fe-579a93c01e19/Untitled.png)

1. **Конфигурация транспорта:**

### 4.1 U(S)ART с прерываниями

Шаги для настройки:

1. Включите U(S)ART в вашем STM32CubeIDE.
2. Для выбранного USART включите глобальное прерывание в настройках NVIC.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/976fc81f-96cc-4b9e-9ce6-33873c9dc8a8/Untitled.png)

### 4.2 U(S)ART с DMA

Шаги для настройки:

1. Включите U(S)ART в вашем STM32CubeIDE.
2. Для выбранного USART включите **DMA для Tx и Rx** в настройках DMA.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/2aa0bd20-3406-4248-91c0-85add8b4a8ec/Untitled.png)

1. Установите приоритет DMA на **"Very High"** для Tx и Rx.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/8a5c10d8-79c7-4d75-ad67-52c138b84487/Untitled.png)

1. Установите режим DMA на **"Circular"** для **Rx**. **(Для TX НЕ НАДО)**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/1465871f-3e14-4675-abd2-b05adf619de7/Untitled.png)

1. Для выбранного **USART** включите глобальное прерывание в настройках **NVIC**.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/e0ae7bb0-afd6-44b4-a1bc-7d511afb94bc/Untitled.png)

### 4.3 USB CDC

Шаги для настройки:

1. Включите USB в вашем STM32CubeIDE на вкладке **Connectivity**.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/38d9f403-b231-4ccd-8e83-761fcaf855ed/Untitled.png)

1. Выберите режим **Communication Device Class (Virtual Port Com)** в настройках **Middleware -> USB_DEVICE**.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/32be508a-69c2-4783-861f-cc1bd059bd82/Untitled.png)

**Примечание: транспорт micro-ROS надо настроить USB_DEVICE/App/usbd_cdc_if.c.**

1. **FreeRTOS**

Убедитесь, что если вы используете FreeRTOS, задача micro-ROS имеет более **10 кБ** стека.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/94a74835-e4a4-486b-b274-24a90f40a113/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/2ef5ad17-f87c-409d-9d9c-55c6ccf83fe5/Untitled.png)

# 2. Скачивание micro-ros библиотек.

1. Клонируйте с гита репозиторий  в папку проекта STM32CubeIDE.

```bash
git clone https://github.com/micro-ROS/micro_ros_stm32cubemx_utils.git
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/c418dd30-9e12-45e3-9c70-6dd1d31d67bf/Untitled.png)

1. Перейдите в Project -> Settings -> C/C++ Build -> Settings -> Build Steps Tab и в Pre-build steps добавьте:

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/bef697c5-c43c-48c1-b436-02fd5bfe89d2/Untitled.png)

```bash
docker pull microros/micro_ros_static_library_builder:foxy && docker run --rm -v ${workspace_loc:/${ProjName}}:/project --env MICROROS_LIBRARY_FOLDER=micro_ros_stm32cubemx_utils/microros_static_library_ide microros/micro_ros_static_library_builder:foxy
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/2409b1bb-0295-4e41-b4ff-d92bbb1d0259/Untitled.png)

1. Добавьте директорию включаемых файлов micro-ROS. В Project -> Settings -> C/C++ Build -> Settings -> Tool Settings Tab -> MCU GCC Compiler -> Include paths добавьте 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/1d08947a-a7b8-4f4f-9b76-f23937b016a9/Untitled.png)

```bash
../micro_ros_stm32cubemx_utils/microros_static_library_ide/libmicroros/include
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/843123b0-1bae-46f4-8e38-724b2d7fc7a0/Untitled.png)

1. Добавьте предварительно скомпилированную библиотеку micro-ROS. В Project -> Settings -> C/C++ Build -> Settings -> MCU GCC Linker -> Libraries: (Если возникнуть проблемы с докером, во время build **[
Проблема связанное с docker](https://www.notion.so/docker-cd842045d9404e7088a19e28e609aa00?pvs=21)** )

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/3a0744a3-d229-4589-b7fb-ea02171c6a3d/Untitled.png)

- добавьте `<ABSOLUTE_PATH_TO>/micro_ros_stm32cubemx_utils/microros_static_library_ide/libmicroros` в Library search path (-L)

```bash
../micro_ros_stm32cubemx_utils/microros_static_library_ide/libmicroros
```

- добавьте `microros` в Libraries (-l)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4c38828c-5eeb-4a34-b220-4bd44313491b/e7913f92-84bf-4b1e-ac2e-a2056454cd09/Untitled.png)

1. Добавьте следующие исходные файлы в ваш проект, перетащив их в папку source:
    - `extra_sources/microros_time.c`
    - `extra_sources/microros_allocators.c`
    - `extra_sources/custom_memory_manager.c`
    - `extra_sources/microros_transports/dma_transport.c` или выбранный вами транспорт.
2. Соберите и запустите ваш проект.

### Настройка библиотеки micro-ROS

Вся настройка micro-ROS может быть выполнена в файле colcon.meta перед шагом 3. Подробную информацию о том, как настроить использование статической памяти библиотеки, можно найти в руководстве по настройке Middleware.

### Добавление пользовательских пакетов

Обратите внимание, что папки, добавленные в microros_static_library/library_generation/extra_packages/, и записи, добавленные в microros_static_library/library_generation/extra_packages/extra_packages.repos, будут учитываться этой системой сборки.

# Проблема связанное с docker

### Решение проблемы "permission denied while trying to connect to the Docker daemon socket"

Если при попытке выполнения команд Docker вы получаете ошибку "permission denied while trying to connect to the Docker daemon socket", следуйте этим шагам, чтобы исправить проблему.

### Шаг 1: Добавление пользователя в группу Docker

Добавьте текущего пользователя в группу Docker, чтобы он мог выполнять команды Docker без `sudo`:

1. Откройте терминал.
2. Выполните команду:
    
    ```bash
    sudo usermod -aG docker $USER
    
    ```
    

### Шаг 2: Перезапуск службы Docker

Перезапустите службу Docker, чтобы изменения вступили в силу:

1. Выполните команду:
    
    ```bash
    sudo systemctl restart docker
    
    ```
    

### Шаг 3: Проверка добавления пользователя в группу Docker

Убедитесь, что текущий пользователь действительно добавлен в группу Docker:

1. Выполните команду:
Вы должны увидеть `docker` в списке групп.
    
    ```bash
    groups $USER
    
    ```
    

### Шаг 4: Выход и вход в систему

Выйдите из системы и войдите снова, чтобы изменения вступили в силу.

### Шаг 5: Перезагрузка системы (если необходимо)

Если после выхода и входа в систему проблема сохраняется, перезагрузите систему:

1. Выполните команду:
    
    ```bash
    sudo reboot
    
    ```
    

### Шаг 6: Проверка Docker

После выполнения всех вышеуказанных шагов, проверьте, работает ли Docker без `sudo`:

1. Откройте терминал.
2. Выполните команду:
    
    ```bash
    docker run hello-world
    
    ```