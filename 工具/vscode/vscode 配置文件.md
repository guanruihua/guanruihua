```json
{
	"window.menuBarVisibility": "compact",
	"vim.easymotion": true,
	"vim.incsearch": true,
	"vim.useSystemClipboard": true,
	"vim.useCtrlKeys": true,
	"vim.hlsearch": true,
	"vim.insertModeKeyBindings": [
		// { "before": [ "", "" ], "after": [ "<Esc>" ] }
	],
	// "vim.normalModeKeyBindingsNonRecursive": [
	// 	{ "before": [ "<leader>", "d" ], "after": [ "d", "d" ] },
	// 	{ "before": [ "<C-n>" ], "commands": [ ":nohl" ] }
	// ],
	// "vim.leader": "<space>",
	"vim.handleKeys": {
		"<C-a>": false,
		"<C-f>": true,
		"<C-c>": false,
		"<C-x>": false,
		"<C-d>": false,
		"<C-j>": false,
		"<C-p>": false,
		"<C-w>": false,
		"<C-o>": false,
	}, // 活动栏的显隐
	"workbench.startupEditor": "newUntitledFile",
	// "editor.minimap.enabled":true, // 缩略图
	"extensions.ignoreRecommendations": true,
	"editor.insertSpaces": false,
	"editor.suggestSelection": "first",
	"explorer.confirmDelete": true,
	"explorer.confirmDragAndDrop": false,
	"js/ts.implicitProjectConfig.experimentalDecorators": true,
	"javascript.updateImportsOnFileMove.enabled": "always",
	"editor.linkedEditing": true,
	"git.enableSmartCommit": true,
	"editor.renderControlCharacters": true,
	"database-client.defaultSelectLimit": 30,
	"editor.fontSize": 11,
	"vetur.ignoreProjectWarning": true,
	// atom
	// Controls whether the prompt will show
	"atomKeymap.promptV3Features": true,
	// Changes the multi cursor mouse binding
	"editor.multiCursorModifier": "ctrlCmd",
	// Controls whether format on paste is on or off
	"editor.formatOnPaste": true,
	"[javascript]": {
		"editor.defaultFormatter": "vscode.typescript-language-features"
	},
	"[html]": {
		"editor.defaultFormatter": "vscode.html-language-features"
	},
	"git.autofetch": true,
	"editor.codeActionsOnSave": {
		"source.fixAll.markdownlint": true,
	},
	"editor.tokenColorCustomizations": {
		"comments": "#7ED321", //注释颜色
		"statusBar.background": "#1A1A1A",
		"statusBar.noFolderBackground": "#0A0A0D",
		"statusBar.debuggingBackground": "#511f1f",
	},
	"workbench.colorCustomizations": {
		"editorCursor.foreground": "#fa0c0c", // 设置光标颜色
		"statusBar.background": "#1e1e1e",
		"activityBar.activeBackground": "#1e1e1e",
		"activityBar.background": "#1e1e1e",
		"sideBar.background": "#1e1e1e",
		"editorGroup.border": "#1e3",
		"panel.background": "#1e1e1e",
		"editorGroup.dropBackground": "#1e1e1e",
		"editorGroupHeader.tabsBackground": "#1e1e1e",
		// "statusBar.background": "#323232",
		"statusBar.noFolderBackground": "#0A0A0D",
		"statusBar.debuggingBackground": "#511f1f",
		// "activityBar.activeBackground": "#323232",
		// "activityBar.background": "#323232",
		// "activityBar.foreground": "#ffffff",
		// "sideBar.background": "#323232",
		// "editorGroup.dropBackground": "#323232",
		// "editorGroupHeader.tabsBackground": "#323232",
		// "panel.background": "#323232",
		// "list.activeSelectionBackground": "#37373d",
		"list.activeSelectionBackground": "#1e1e1e",
		"editor.background": "#1e1e1e",
	},
	"update.showReleaseNotes": false,
	"telemetry.enableCrashReporter": false,
	"update.enableWindowsBackgroundUpdates": false,
	"scss.lint.fontFaceProperties": "ignore",
	"less.lint.fontFaceProperties": "ignore",
	"css.lint.fontFaceProperties": "ignore",
	"terminal.integrated.fontFamily": "宋体",
	"terminal.integrated.fontWeight": "normal",
	"[json]": {
		"editor.defaultFormatter": "vscode.json-language-features"
	},
	"liveServer.settings.donotShowInfoMsg": true,
	"search.followSymlinks": false,
	"terminal.integrated.cursorStyle": "line",
	// "vim.cursorStylePerMode.insert": "line",
	"editor.cursorStyle": "line",
	"workbench.editorAssociations": {
		"*.ipynb": "jupyter.notebook.ipynb"
	},
	"python.linting.flake8Enabled": true,
	"python.formatting.provider": "yapf",
	"python.linting.flake8Args": [
		"--max-line-length=248"
	],
	/* prettier start */
	"editor.formatOnSave": true, // 保存后格式化
	// "editor.formatOnSave": false, // 保存后格式化
	"editor.formatOnType": true,
	"emmet.includeLanguages": {
		"javascript": "javascriptreact"
	},
	"[jsonc]": {
		"editor.defaultFormatter": "vscode.json-language-features"
	},
	"[vue]": {
		"editor.defaultFormatter": "esbenp.prettier-vscode"
	},
	"[typescriptreact]": {
		"editor.defaultFormatter": "esbenp.prettier-vscode"
	},
	"[typescript]": {
		"editor.defaultFormatter": "esbenp.prettier-vscode"
	},
	"[javascriptreact]": {
		"editor.defaultFormatter": "vscode.typescript-language-features"
	},
	"editor.defaultFormatter": "esbenp.prettier-vscode",
	"prettier.useTabs": true,
	"prettier.requireConfig": true,
	"prettier.arrowParens": "avoid",
	"prettier.jsxSingleQuote": true,
	"prettier.insertPragma": true,
	"prettier.jsxBracketSameLine": true,
	"prettier.semi": false,
	"prettier.singleQuote": true,
	"prettier.useEditorConfig": false, // 优先使用editorconfig配置文件
	/* prettier end */
	//使用制图符缩进
	"terminal.integrated.scrollback": 100, // 终端保存的行数
	"editor.maxTokenizationLineLength": 204800, // 对长航跳过令牌化
	"files.associations": {
		"*.json": "jsonc",
	},
	"editor.wordSeparators": "/\\()\"':,.;<>~!@#$%^&*|+=[]{}`?-",
	"editor.tabSize": 2,
	"editor.wordWrap": "on",
	"diffEditor.wordWrap": "on",
	"typescript.suggest.paths": false,
	"editor.lineNumbers": "relative",
	"editor.minimap.enabled": false,
	/* file header start */
	"psi-header.variables": [
		[
			"author",
			"ruihuag"
		]
	],
	"psi-header.templates": [
		{
			"language": "*",
			"template": [
				"File: <<filename>>",
				"Project: <<projectname>>",
				"Author: <<author>>",
				"File Created: <<filecreated('dddd, Do MMMM YYYY h:mm:ss a')>>",
				"Modified By: <<author>>",
				"Last Modified: <<dateformat('dddd, Do MMMM YYYY h:mm:ss a')>>",
			]
		}
	],
	"psi-header.changes-tracking": {
		"isActive": true,
		"modAuthor": "Modified By: ",
		"modDate": "Last Modified: ",
		"modDateFormat": "date",
		"include": [],
		"exclude": [
			"markdown",
			"json",
		],
		"excludeGlob": [
			"out/**",
			"src/**/*.xyz",
			"components/**/*.mk",
		],
		"autoHeader": "manualSave",
		// "autoHeader": "autoSave",
	},
	/* file header end */
	"files.exclude": {
		"**/.keep": true,
		// "**/node_modules": false,
		"**/node_modules": true,
		"**/yarn.lock": true
	},
	"breadcrumbs.filePath": "off", // 面包屑
	"breadcrumbs.symbolPath": "off",
	"breadcrumbs.enabled": false,
	"javascript.suggest.autoImports": false,
	"telemetry.enableTelemetry": false,
	"php.validate.enable": false,
	"database-client.highlightSQLBlock": true,
	"security.workspace.trust.emptyWindow": false,
	"security.workspace.trust.startupPrompt": "never",
	"workbench.iconTheme": null,

	"workbench.editor.enablePreviewFromQuickOpen": false,
	"workbench.editor.enablePreview": false,

	"editor.formatOnSaveMode": "modifications",
}
```

