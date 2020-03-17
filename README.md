    private class ViewOnclickListener implements OnClickListener {
        Intent intent = new Intent();

        @Override
        public void onClick(View v) {
            switch (v.getId()) {
                case R.id.play_music:
                    if (isPlaying) {
                        playBtn.setBackgroundResource(R.drawable.pause_selector);
                        intent.setAction("com.wwj.media.MUSIC_SERVICE");
                        intent.putExtra("MSG", AppConstant.PlayerMsg.PAUSE_MSG);
                        startService(intent);
                        isPlaying = false;
                        isPause = true;
                    } else if (isPause) {
                        playBtn.setBackgroundResource(R.drawable.play_selector);
                        intent.setAction("com.wwj.media.MUSIC_SERVICE");
                        intent.putExtra("MSG", AppConstant.PlayerMsg.CONTINUE_MSG);
                        startService(intent);
                        isPause = false;
                        isPlaying = true;
                    }
                    break;
                case R.id.previous_music: // 上一首歌曲
                    previous_music();
                    break;
                case R.id.next_music: // 下一首歌曲
                    next_music();
                    break;
                case R.id.repeat_music: // 重复播放音乐
                    if (repeatState == isNoneRepeat) {
                        repeat_one();
                        shuffleBtn.setClickable(false); // 是随机播放变为不可点击状态
                        repeatState = isCurrentRepeat;
                    } else if (repeatState == isCurrentRepeat) {
                        repeat_all();
                        shuffleBtn.setClickable(false);
                        repeatState = isAllRepeat;
                    } else if (repeatState == isAllRepeat) {
                        repeat_none();
                        shuffleBtn.setClickable(true);
                        repeatState = isNoneRepeat;
                    }
                    Intent intent = new Intent(REPEAT_ACTION);
                    switch (repeatState) {
                        case isCurrentRepeat: // 单曲循环
                            repeatBtn
                                    .setBackgroundResource(R.drawable.repeat_current_selector);
                            Toast.makeText(PlayerActivity.this,
                                    R.string.repeat_current, Toast.LENGTH_SHORT).show();

                            intent.putExtra("repeatState", isCurrentRepeat);
                            sendBroadcast(intent);
                            break;
                        case isAllRepeat: // 全部循环
                            repeatBtn
                                    .setBackgroundResource(R.drawable.repeat_all_selector);
                            Toast.makeText(PlayerActivity.this, R.string.repeat_all,
                                    Toast.LENGTH_SHORT).show();
                            intent.putExtra("repeatState", isAllRepeat);
                            sendBroadcast(intent);
                            break;
                        case isNoneRepeat: // 无重复
                            repeatBtn
                                    .setBackgroundResource(R.drawable.repeat_none_selector);
                            Toast.makeText(PlayerActivity.this, R.string.repeat_none,
                                    Toast.LENGTH_SHORT).show();
                            intent.putExtra("repeatState", isNoneRepeat);
                            break;
                    }
                    break;
                case R.id.shuffle_music: // 随机播放状态
                    Intent shuffleIntent = new Intent(SHUFFLE_ACTION);
                    if (isNoneShuffle) { // 如果当前状态为非随机播放，点击按钮之后改变状态为随机播放
                        shuffleBtn
                                .setBackgroundResource(R.drawable.shuffle_selector);
                        Toast.makeText(PlayerActivity.this, R.string.shuffle,
                                Toast.LENGTH_SHORT).show();
                        isNoneShuffle = false;
                        isShuffle = true;
                        shuffleMusic();
                        repeatBtn.setClickable(false);
                        shuffleIntent.putExtra("shuffleState", true);
                        sendBroadcast(shuffleIntent);
                    } else if (isShuffle) {
                        shuffleBtn
                                .setBackgroundResource(R.drawable.shuffle_none_selector);
                        Toast.makeText(PlayerActivity.this, R.string.shuffle_none,
                                Toast.LENGTH_SHORT).show();
                        isShuffle = false;
                        isNoneShuffle = true;
                        repeatBtn.setClickable(true);
                        shuffleIntent.putExtra("shuffleState", false);
                        sendBroadcast(shuffleIntent);
                    }
                    break;

                case R.id.ibtn_player_voice:    //控制音量
                    voicePanelAnimation();
                    break;
                case R.id.play_queue:
                    showPlayQueue();
                    break;
            }
        }
    }

