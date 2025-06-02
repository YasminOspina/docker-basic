pipeline {
    agent any
    
    tools {
        dotnetsdk 'dotnet-9'
        nodejs 'node-22'
    }

    environment {
        DOTNET_ROOT = "${env.PATH}:${tool 'dotnet-9'}/bin"
        PATH = "${env.PATH}:${tool 'node-22'}/bin"
    }
}
