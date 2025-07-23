
# Tutorial em Português: Configuração Híbrida de GPU (Intel e NVIDIA) no Arch Linux usando Hyprland

## Etapa 1: Instalar os drivers necessários

### 1.1 Instalar os drivers Intel

Para a GPU Intel, execute:

```bash
sudo pacman -S mesa vulkan-intel libva-intel-driver intel-media-driver
```

### 1.2 Instalar os drivers NVIDIA

Siga o guia passo a passo na wiki do Hyprland:

https://wiki.hyprland.org/Nvidia/

## Etapa 2: Configuração do GRUB para suportar GPUs

### 2.1 Edite o arquivo GRUB `/etc/grub.d/40_custom`

```bash
sudo nano /etc/grub.d/40_custom
```

### 2.2 Adicione o seguinte conteúdo, substituindo o UUID pela partição raiz do seu sistema:

```bash
menuentry "Arch Linux - NVIDIA" {
    search --fs-uuid --set=root <seu_uuid>
    linux /vmlinuz-linux root=UUID=<seu_uuid> rw loglevel=3 quiet nvidia-drm.modeset=1
    initrd /initramfs-linux.img
}
```

Para encontrar o UUID das partições do seu sistema no Linux, você pode usar o comando:

```bash
lsblk -o NAME,FSTYPE,LABEL,UUID,MOUNTPOINTS
```

### 2.3 Após a etapa 2.2, execute o comando para tornar o arquivo de configuração executável:

```bash
sudo chmod +x /etc/grub.d/40_custom
```

### 2.4 Agora, gere a nova configuração do GRUB:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

No GRUB, aparecerá a opção:

* Arch Linux - NVIDIA → Para inicializar usando a placa gráfica dedicada do seu notebook (para evitar travamentos e engasgos em um monitor externo — observação: isso me ajudou a evitar travamentos no monitor externo ao usar Arch Linux com Hyprland)

## Etapa 3: Verificando a Configuração

Após configurar, reinicie o sistema e verifique se tudo está funcionando corretamente.

Use o seguinte comando para verificar qual GPU está sendo usada:

```bash
glxinfo | grep "OpenGL renderer"
```

Se o pacote estiver ausente, instale o `mesa-utils` com:

```bash
sudo pacman -S mesa-utils
```

Exemplo de saída do GRUB:

1. Para NVIDIA: OpenGL renderer string: NVIDIA GeForce RTX 3050 ...
2. Para Intel: OpenGL renderer string: Mesa Intel(R) Graphics ...

## Observação

Se você quiser usar a NVIDIA para aplicações gráficas pesadas enquanto mantém a Intel para tarefas leves, pode usar o **PRIME Render Offload**. Para rodar um aplicativo com a NVIDIA, use o seguinte comando:

```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia <nome-do-aplicativo>
```

Para mais informações, visite: https://wiki.archlinux.org/title/PRIME

# Tutorial in English: Hybrid GPU Configuration (Intel and NVIDIA) on Arch Linux using Hyprland

## Step 1: Install the necessary drivers

### 1.1 Install Intel drivers

For the Intel GPU, run:

```bash
sudo pacman -S mesa vulkan-intel libva-intel-driver intel-media-driver
```

### 1.2 Install NVIDIA drivers

Follow the step-by-step guide on the Hyprland wiki:

https://wiki.hyprland.org/Nvidia/

## Step 2: GRUB Configuration to Support GPUs

### 2.1 Edit the GRUB file `/etc/grub.d/40_custom`

```bash
sudo nano /etc/grub.d/40_custom
```

### 2.2 Add the following content, replacing the UUID with your root partition:

```bash
menuentry "Arch Linux - NVIDIA" {
    search --fs-uuid --set=root <your_uuid>
    linux /vmlinuz-linux root=UUID=<your_uuid> rw loglevel=3 quiet nvidia-drm.modeset=1
    initrd /initramfs-linux.img
}
```

To find the UUID of your system partitions on Linux, you can use the command:

```bash
lsblk -o NAME,FSTYPE,LABEL,UUID,MOUNTPOINTS
```

### 2.3 After step 2.2, run the command to make the configuration file executable:

```bash
sudo chmod +x /etc/grub.d/40_custom
```

### 2.4 Now, generate the new GRUB configuration:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

In GRUB, the option will appear:

* Arch Linux - NVIDIA → To boot using your laptop's dedicated graphics card (to avoid lags and stutters on an external monitor — note: this helped me prevent external monitor stuttering when using Arch Linux with Hyprland)

## Step 3: Verifying the Configuration

After configuring, restart the system and check if everything is working correctly.

Use the following command to check which GPU is being used:

```bash
glxinfo | grep "OpenGL renderer"
```

If the package is missing, install `mesa-utils` with:

```bash
sudo pacman -S mesa-utils
```

Example GRUB output:

1. For NVIDIA: OpenGL renderer string: NVIDIA GeForce RTX 3050 ...
2. For Intel: OpenGL renderer string: Mesa Intel(R) Graphics ...

## Note

If you want to use NVIDIA for heavy graphics applications while keeping Intel for light tasks, you can use PRIME Render Offload. To run an application with NVIDIA, use the following command:

```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia <app-name>
```

For more information, visit: https://wiki.archlinux.org/title/PRIME
