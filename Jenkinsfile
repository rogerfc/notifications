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
		{
			"type": "header",
			"text": {
				"type": "plain_text",
				"text": "Magento deployment",
				"emoji": true
			}
		},
		{
			"type": "section",
			"fields": [
				{
					"type": "mrkdwn",
					"text": "*Environment*:\nUAT"
				},
				{
					"type": "mrkdwn",
					"text": "*Release*:\n10.4"
				}
			]
		},
		{
			"type": "section",
			"text": {
				"type": "mrkdwn",
				"text": "Jenkins build"
			},
			"accessory": {
				"type": "button",
				"text": {
					"type": "plain_text",
					"text": "View",
					"emoji": true
				},
				"url": "https://jenkins-i4.ackstorm.net/job/fluidra-orion-test-pipelines/job/test-pipelines/4/"
			}
		},
		{
			"type": "divider"
		},
		{
			"type": "context",
			"elements": [
				{
					"type": "mrkdwn",
					"text": ":runner:  *2020-12-10 09:45* START"
				}
			]
		},
		{
			"type": "context",
			"elements": [
				{
					"type": "mrkdwn",
					"text": ":white_check_mark:  *2020-12-10 09:45* SUCCESS"
				}
			]
		},
		{
			"type": "context",
			"elements": [
				{
					"type": "mrkdwn",
					"text": ":poop:  *2020-12-10 09:45* FAIL"
				}
			]
		}
	]
    def slackResponse = slackSend (
        channel: channel,
        // attachments: attachments,
        blocks: blocks
    )
    return slackResponse
}



// slackSend(channel: "#general", blocks: blocks)
