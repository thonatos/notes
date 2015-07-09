/**
 * Created by thonatos on 14/12/17.
 */

var html5Player = {

    create: function ($container, $video, $player, options) {

        var obj = {};
        var _protected = {};

        /**
         * Variable
         * --------
         *
         * $container
         * $video
         * $player
         *
         * options
         *      play
         *      mute
         *      progress
         *      volume
         *      fullScreen
         *
         * _dataUrl
         *
         */

        var $container = $container;
        var $video = $video;
        var $player = $player;

        var defaultOptions =  {
            videoUrl: '',
            display: {
                all:true,
                playControl: true,
                muteControl: true,
                fullScreenControl: true,
                seekControl: true,
                volumeControl: true
            },
            control: {
                autoplay:true,
                preload:true,
                loop:true,
                progress: false,
                volume: false
            }
        };

        var currentOptions = {};

        if (options) {
            for (var key in options) {
                if (key in defaultOptions) {
                    defaultOptions[key] = options[key];
                }
            }
        }

        currentOptions = defaultOptions;

        var PLAYER_PROPORTION = 16 / 9;

        var _dataUrl = options.videoUrl || '';

        _protected.updateContainerSize = function () {

            var width = $container.width();
            var height = width / PLAYER_PROPORTION;

            //$container.find('.preview,video').css({
            //    "width": width,
            //    "height": height
            //});
        };

        _protected.addPlayerControl = function () {

            var _controls = '';

            _controls += '<div class="video-controls">';
            _controls += '<div class="controls-container">';
            _controls += '<div class="play">';
            _controls += '<a class="button" id="play-pause"></a>';
            _controls += '</div>';
            _controls += '<div class="progress">';
            _controls += '<input type="range" id="seek-bar" class="seek-bar" value="0">';
            _controls += '</div>';
            _controls += '<div class="mute">';
            _controls += '<a class="button" id="mute"></a>';
            _controls += '</div>';
            _controls += '<div class="volume">';
            _controls += '<input type="range" id="volume-bar" class="volume-bar" min="0" max="1" step="0.1" value="1">';
            _controls += '</div>';
            _controls += '<div class="full-screen">';
            _controls += '<a class="button" id="full-screen"></a>';
            _controls += '</div>';
            _controls += '</div>';
            _controls += '</div>';

            $container.append(_controls);
        };

        _protected.updateControlStyle = function () {

            // Container
            var _videoWidth = $container.find('video').width();
            var _left = 0;

            // hide volume and mute
            if (!currentOptions.display.volumeControl) {
                $container.find('.video-controls .mute').hide();
                $container.find('.video-controls .volume').hide();
            }

            // show / hide volume
            if (currentOptions.display.volumeControl) {

                _left = _videoWidth - 36 * 3 - 4 * 5;

                $container.find('.progress').css({
                    'width': _left * 0.6
                });

                $container.find('.volume').css({
                    'width': _left * 0.35
                });
            } else {

                _left = _videoWidth - 36 * 2 - 4 * 3;

                $container.find('.progress').css({
                    'width': _left * 0.99
                });
            }

        };

        _protected.controlHandler = function () {

            /**
             * Event
             * --------
             * playButton.play-pause.click
             * muteButton.mute.click
             * fullScreenButton.full-screen.click
             *
             * seekBar.change
             * volumeBar.change
             */

            if (!currentOptions.display.all) {
                document.getElementsByClassName('video-controls')[0].style.display='none';
            }

            // Event listener for the play/pause button
            if (currentOptions.display.playControl) {

                var playButton = document.getElementById("play-pause");

                playButton.addEventListener("click", function () {
                    if ($player.paused == true) {
                        $player.play();
                    } else {
                        $player.pause();
                    }
                });
            }

            // Event listener for the mute button
            if (currentOptions.display.muteControl) {

                var muteButton = document.getElementById("mute");

                muteButton.addEventListener("click", function () {
                    if ($player.muted == false) {
                        $player.muted = true;
                        //muteButton.innerHTML = "Unmute";
                        muteButton.style.backgroundImage = 'url(/public/images/player/player-mute.svg)';
                    } else {
                        $player.muted = false;
                        //muteButton.innerHTML = "Mute";
                        muteButton.style.backgroundImage = 'url(/public/images/player/player-unmute.svg)';
                    }
                });
            }

            // Event listener for the full-screen button
            if (currentOptions.display.fullScreenControl) {

                var fullScreenButton = document.getElementById("full-screen");

                fullScreenButton.addEventListener("click", function () {
                    console.log('RequestFullScreen');
                    if ($player.requestFullScreen) {
                        $player.requestFullScreen();
                    } else if ($player.mozRequestFullScreen) {
                        $player.mozRequestFullScreen(); // Firefox
                    } else if ($player.webkitRequestFullScreen) {
                        $player.webkitRequestFullScreen(); // Chrome and Safari
                    } else if ($player.oRequestFullScreen) {
                        $player.oRequestFullScreen(); // Chrome and Safari
                    } else if ($player.msRequestFullScreen) {
                        $player.msRequestFullScreen(); // Chrome and Safari
                    }
                });
            }

            // Event listener for the seek bar
            if (currentOptions.display.seekControl) {

                var seekBar = document.getElementById("seek-bar");

                seekBar.addEventListener("change", function () {
                    var _time = $player.duration * (seekBar.value / 100);

                    if (currentOptions.control.progress) {
                        if ($player.seekable) {
                            $player.currentTime = _time;
                        } else {
                            seekTo(_time);
                        }
                    } else {
                        // LIVE
                        console.log('Live, no seekable');
                    }

                    function seekTo(value) {
                        if ($player.seekable) {
                            $player.currentTime = value;

                            return false;
                        } else {
                            setTimeout(function () {
                                seekTo(value);
                            }, 500)
                        }
                    }
                });
            }

            // Event listener for the volume bar
            if (currentOptions.display.volumeControl) {

                var volumeBar = document.getElementById("volume-bar");

                volumeBar.addEventListener("change", function () {
                    $player.volume = volumeBar.value;
                });
            }
        };

        _protected.playerHandler = function () {

            //Add listener handler
            $player.addEventListener('play', playHandler, false);
            $player.addEventListener('pause', pauseHandler, false);
            $player.addEventListener('playing', playingHandler, false);
            $player.addEventListener('timeupdate', timeUpdateHandler, false);
            $player.addEventListener('touchstart', touchStartHandler, false);
            $player.addEventListener('touchend', touchEndHandler, false);

            /**
             * Handler
             * --------
             *
             * playingHandler
             * playHandler
             * pauseHandler
             * timeupdateHandler
             *
             * toggleFullScreen
             * toggleMute
             *
             * timeupdate
             *
             * touchStartHandler
             * touchEndHandler
             *
             */

            function playingHandler() {

                console.log(getTime() + ' Player.Playing');

                if (currentOptions.display.playControl) {
                    var playButton = document.getElementById("play-pause");
                    playButton.style.backgroundImage = 'url(/public/images/player/player-pause.svg)';
                }

            }

            function pauseHandler() {

                console.log(getTime() + ' Player.Pause');

                if (currentOptions.display.playControl) {
                    var playButton = document.getElementById("play-pause");
                    playButton.style.backgroundImage = 'url(/public/images/player/player-play.svg)';
                }

            }

            function playHandler() {
                console.log(getTime() + ' Player.Play');
            }

            function timeUpdateHandler() {
                //console.log(getTime() + ' Player.timeUpdate');
                var _value = (100 / $player.duration) * $player.currentTime;

                if (currentOptions.display.seekControl) {
                    var seekBar = document.getElementById("seek-bar");
                    seekBar.value = _value;
                }
            }

            function touchStartHandler() {
                console.log(getTime() + ' Player.touchStart');

                document.getElementsByClassName('video-controls')[0].style.opacity = '0.9';
            }

            function touchEndHandler() {
                console.log(getTime() + ' Player.touchEnd');
                setTimeout(function () {
                    document.getElementsByClassName('video-controls')[0].style.opacity = '0';
                }, 2000);
            }

            function getTime() {
                return (new Date().getTime());
            }
        };

        _protected.updateVideoSize = function (elem,width, height) {

            // From Covervid
            // https://github.com/stefanerickson/covervid

            // call sizeVideo on load
            document.addEventListener('DOMContentLoaded', sizeVideo,false);

            // call sizeVideo on resize
            window.onresize = function () {
                debounce(sizeVideo(), 50);
            };

            // debounce for resize function
            function debounce(fn, delay) {
                var timer = null;

                return function () {
                    var context = this,
                        args = arguments;

                    window.clearTimeout(timer);

                    timer = window.setTimeout(function () {
                        fn.apply(context, args);
                    }, delay);
                };
            }

            // Set necessary styles to position video "center center"
            elem.style.position = 'absolute';
            elem.style.top = '50%';
            elem.style.left = '50%';
            elem.style['-webkit-transform'] = 'translate(-50%, -50%)';
            elem.style['-ms-transform'] = 'translate(-50%, -50%)';
            elem.style.transform = 'translate(-50%, -50%)';

            // Set overflow hidden on parent element
            elem.parentNode.style.overflow = 'hidden';

            // Define the attached selector
            function sizeVideo() {

                console.log(new Date());

                // Get parent element height and width
                var parentHeight = elem.parentNode.offsetHeight;
                var parentWidth = elem.parentNode.offsetWidth;

                // Get native video width and height
                var nativeWidth = width;
                var nativeHeight = height;

                // Get the scale factors
                var heightScaleFactor = parentHeight / nativeHeight;
                var widthScaleFactor = parentWidth / nativeWidth;

                // Based on highest scale factor set width and height
                if (widthScaleFactor > heightScaleFactor) {
                    elem.style.height = 'auto';
                    elem.style.width = parentWidth+'px';
                } else {
                    elem.style.height = parentHeight+'px';
                    elem.style.width = 'auto';
                }
            }
        };

        /**
         *
         * @param videoUrl
         * @param callback
         */
        _protected.updateVideoUrl = function (playing, videoUrl, callback) {


            /**
             * 是否正在播放？
             * 是，更换视频流，并回放到原始位置；
             * 否，更新视频流，并等待用户点击开始。
             *
             * 返回信息playing:
             *      true: 播放
             *      false：未播放
             */

            if (playing) {

                // 视频已播放
                var _currentTime = $player.currentTime.toFixed(1);
                $player.src = videoUrl;
                $player.load();
                $player.play();
                seekTo(_currentTime);

                // Add listener to auto hide event-info
                $player.addEventListener("playing", playingHandler, false);
            } else {

                // 视频未播放
                $player.src = videoUrl;
                $player.load();
                callback(false);
            }


            function playingHandler() {
                console.log("updateVideoUrl.playingHandler");
                $player.removeEventListener("playing", playingHandler, false);
            }

            function seekTo(value) {

                var _SEEK_DELAY = 500;

                if ($player.readyState == 0) {

                    setTimeout(function () {
                        seekTo(value);
                    }, _SEEK_DELAY * 2);
                    return false;

                } else {

                    if (($player.currentTime !== null) && ($player.duration > value)) {
                        console.log('TO SEEK');
                        $player.currentTime = value;
                        callback(true);
                    } else {
                        console.log('Wait for data ... biu biu biu .');
                        setTimeout(function () {
                            seekTo(value);
                        }, _SEEK_DELAY);
                        return false;
                    }
                }
            }
        };

        /**
         * obj
         * --------
         *
         * init
         * resume
         * resize
         * updateVideoUrl
         *
         */

        obj.init = function () {

            $video.remove();
            $video.find('source').attr('src', _dataUrl);

            // ADD ATTRIBUTE
            if(currentOptions.control.autoplay){
                $video.attr('autoplay', 'autoplay');
            }

            if(currentOptions.control.preload){
                $video.attr("preload", "auto");
            }

            if(currentOptions.control.loop){
                $video.attr("loop", "loop");
            }

            $container.prepend($video);
            _protected.updateContainerSize();
            _protected.addPlayerControl();
            _protected.updateControlStyle();
        };

        obj.resume = function () {

            $player.src = _dataUrl;
            $player.load();
            _protected.updateVideoSize($player,1280,720);
            _protected.controlHandler();
            _protected.playerHandler();

        };

        obj.updateVideoUrl = _protected.updateVideoUrl;

        return obj;
    }
};

exports.create = html5Player.create;