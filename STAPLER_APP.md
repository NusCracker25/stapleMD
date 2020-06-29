to fix
had to deactivate Ivvy compiler because of ngx-socketio.
watch over an update of the package

My temporary solution was to add:
````json
"enableIvy": false,

"angularCompilerOptions": {
"fullTemplateTypeCheck": true,
"enableIvy": false, <---
"strictInjectionParameters": true
}
````
from my file tsconfig.json