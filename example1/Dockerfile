FROM node

RUN npm install -g webpack webpack-dev-server yarn

LABEL "org.stowage.spec"="{ \
    \"Name\": \"webpack\", \
    \"Command\": \"webpack\", \
    \"Mounts\": [ \
        { \
            \"Cwd\": true, \
            \"Guest\": \"/workspace\" \
        } \
    ] \
}"

VOLUME /workspace
