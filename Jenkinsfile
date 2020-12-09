pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sendNotification('STARTED')
            }
        }
    }
}

def sendNotification(buildResult, channel = 'area51', fields = [], text = '') {
    def color
    switch(buildResult) {
        case 'STARTED':
            color = 'gray'
            break
        case 'SUCCESS':
            color = 'good'
            break
        case 'FAILURE':
        default:
            color = 'danger'
            break
    }
    if (fields.isEmpty() && text.isEmpty()) {
        switch(buildResult) {
            case 'STARTED':
                text = "${buildResult}"
                break
            default:
                text = "${buildResult} after ${currentBuild.durationString.replace(' and counting', '')}"
        }
    }
    def attachments = [
        [
            color: color,
            title: "${env.JOB_NAME} #${env.BUILD_NUMBER}",
            title_link: env.RUN_DISPLAY_URL,
            fallback: "${env.JOB_NAME} #${env.BUILD_NUMBER} ${buildResult}",
            fields: fields,
            text: text
        ]
    ]
    def blocks = [
    	[
    		"type": "section",
    		"text": [
    			"type": "mrkdwn",
    			"text": "Hello, Assistant to the Regional Manager Dwight! *Michael Scott* wants to know where you'd like to take the Paper Company investors to dinner tonight.\n\n *Please select a restaurant:*"
    		]
    	],
        [
    		"type": "divider"
    	],
    	[
    		"type": "section",
    		"text": [
    			"type": "mrkdwn",
    			"text": "*Farmhouse Thai Cuisine*\n:star::star::star::star: 1528 reviews\n They do have some vegan options, like the roti and curry, plus they have a ton of salad stuff and noodles can be ordered without meat!! They have something for everyone here"
    		],
    		"accessory": [
    			"type": "image",
    			"image_url": "https://s3-media3.fl.yelpcdn.com/bphoto/c7ed05m9lC2EmA3Aruue7A/o.jpg",
    			"alt_text": "alt text for image"
    		]
    	]
    ]
    def slackResponse = slackSend (
        channel: channel,
        // attachments: attachments,
        blocks: blocks
    )
    return slackResponse
}



// slackSend(channel: "#general", blocks: blocks)
