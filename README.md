// ==UserScript==
// @name         Wallpaper Plugin
// @version      0.1
// @description  Define um wallpaper personalizado no BetterDiscord
// @author       VogenZ
// @license      MIT
// ==/UserScript==

const Plugin = class WallpaperPlugin {
    constructor() {
        this.defaultSettings = {
            wallpaperURL: '', // https://i.pinimg.com/564x/30/e5/ec/30e5ec8f1ec47882de36ee53e26fada9.jpg
        };

        this.settings = Object.assign({}, this.defaultSettings);

        this.init();
    }

    init() {
        // Carrega as configurações salvas
        this.loadSettings();

        // Adiciona o comando !wallpaper
        BdApi.commands.register('wallpaper', {
            description: 'Define um wallpaper personalizado',
            usage: '{URL da imagem}',
            executor: (args) => {
                const [wallpaperURL] = args;

                // Define o wallpaper
                this.setWallpaper(wallpaperURL);
            }
        });
    }

    loadSettings() {
        const savedSettings = BdApi.loadData('WallpaperPlugin', 'settings');
        if (savedSettings) {
            this.settings = Object.assign(this.settings, JSON.parse(savedSettings));
        }
    }

    saveSettings() {
        BdApi.saveData('WallpaperPlugin', 'settings', JSON.stringify(this.settings));
    }

    setWallpaper(wallpaperURL) {
        // Define o wallpaper e salva as configurações
        this.settings.wallpaperURL = wallpaperURL;
        this.saveSettings();
        
        // Atualiza a interface para refletir o wallpaper novo
        document.querySelector('.theme-dark').style.backgroundImage = `url('${wallpaperURL}')`;
        document.querySelector('.theme-light').style.backgroundImage = `url('${wallpaperURL}')`;
    }
};

// Adiciona o plugin
window.WallpaperPlugin = new Plugin();

<!---
vogenZ/vogenZ is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
