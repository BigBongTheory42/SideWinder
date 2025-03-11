# SideWinder Script Comments
Comments
(function() {
    'use strict';

    // Constants used throughout the script for configuration
    const CONSTANTS = {
        VERSION: '1.0', // Script version
        SIDEBAR_WIDTH: 425, // Width of the sidebar in pixels
        MIN_GROUP_WIDTH: 180, // Minimum width allowed for groups
        MIN_GROUP_HEIGHT: 100, // Minimum height allowed for groups
        TRADEMARK: '𝕊𝕚𝕕𝕖𝕎𝕚𝕟𝕕𝕖𝕣🔰', // Branding string

        // Various tagline messages displayed randomly in the UI
        TAGLINES: [
            "Made with love by Doobiesuckin [3255641]",
            "Hope you're enjoying the SideWinder Script!",
            "Fixing Torn's UI one script at a time!",
            "You can Drag and Resize Groups in Edit Mode",
            "You can Delete Links, Targets, Groups, and more In Delete Mode",
        ],
        
        // Local storage keys for saving user state
        STATE_KEYS: {
            GROUPS: 'sidebarGroups',
            NOTEPADS: 'sidebarNotepads',
            ATTACK_LISTS: 'attackLists',
            TODO_LISTS: 'todoLists',
            LOAN_TRACKER: 'loanTracker',
            AUCTION_TRACKER: 'auctionTracker',
            COUNTDOWN_GROUPS: 'countdownGroups',
            MANUAL_COUNTDOWN_GROUPS: 'manualCountdownGroups',
            LIGHT_MODE: 'lightMode',
            SIDEBAR_STATE: 'sidebarState',
        },

        // Theme configurations for light and dark mode
        THEMES: {
            LIGHT: {
                BG: '#ffffff', TEXT: '#000000', BORDER: '#cccccc', HEADER: '#e0e0e0', SECONDARY_BG: '#f0f0f0',
                BUTTON_BG: '#666666', SUCCESS: '#336633', DANGER: '#cc3333'
            },
            DARK: {
                BG: '#1a1a1a', TEXT: '#ffffff', BORDER: '#444444', HEADER: '#2c2c2c', SECONDARY_BG: '#333333',
                BUTTON_BG: '#444444', SUCCESS: '#55aa55', DANGER: '#ff5555'
            }
        }
    };

    // State variables storing sidebar data
    let groups = [];
    let notepads = [];
    let attackLists = [];
    let todoLists = [];
    let loanTracker = { entries: [] };
    let auctionTracker = { auctions: [] };
    let countdownGroups = [];
    let manualCountdownGroups = [];
    let isLightMode = false;

    /**
     * Saves a given state key and value to local storage
     * Uses GM_setValue as a fallback if localStorage fails
     * @param {string} key - The state key
     * @param {any} value - The value to save
     */
    function saveState(key, value) {
        try {
            localStorage.setItem(key, JSON.stringify(value));
            localStorage.setItem(`${key}_updatedAt`, Date.now());
        } catch (e) {
            console.error(`Error saving state for ${key}:`, e);
            try {
                GM_setValue(key, JSON.stringify(value));
            } catch (gmError) {
                console.error(`GM_setValue fallback also failed:`, gmError);
            }
        }
    }

    /**
     * Loads a given state key from local storage
     * Uses GM_getValue as a fallback if localStorage fails
     * @param {string} key - The state key
     * @param {any} defaultValue - Default value if the key is not found
     * @returns {any} - The loaded value or default value
     */
    function loadState(key, defaultValue) {
        try {
            const saved = localStorage.getItem(key);
            return saved ? JSON.parse(saved) : defaultValue;
        } catch (e) {
            console.error(`Error loading state for ${key}:`, e);
            return defaultValue;
        }
    }

    /**
     * Initializes stored state variables from local storage
     */
    function initializeState() {
        isLightMode = loadState(CONSTANTS.STATE_KEYS.LIGHT_MODE, false);
        groups = loadState(CONSTANTS.STATE_KEYS.GROUPS, []);
        notepads = loadState(CONSTANTS.STATE_KEYS.NOTEPADS, []);
        attackLists = loadState(CONSTANTS.STATE_KEYS.ATTACK_LISTS, []);
        todoLists = loadState(CONSTANTS.STATE_KEYS.TODO_LISTS, []);
        loanTracker = loadState(CONSTANTS.STATE_KEYS.LOAN_TRACKER, { entries: [] });
        auctionTracker = loadState(CONSTANTS.STATE_KEYS.AUCTION_TRACKER, { auctions: [] });
        countdownGroups = loadState(CONSTANTS.STATE_KEYS.COUNTDOWN_GROUPS, []);
        manualCountdownGroups = loadState(CONSTANTS.STATE_KEYS.MANUAL_COUNTDOWN_GROUPS, []);
    }

    // Initialize the state when script is loaded
    initializeState();
})();
