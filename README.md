# test7seasol.github.io

// Hello World


// View Animation Rotate Icon and expandeable view left to right animation view and left to right animation

    public void rotateIcon(View icon) {
        ObjectAnimator rotateAnimator = ObjectAnimator.ofFloat(icon, "rotation", 0f, 360f);
        rotateAnimator.setDuration(1000); // Duration in milliseconds
        rotateAnimator.setRepeatCount(0); // No repeat
        rotateAnimator.start();
    }


       private void animateHorizontalExpansion(final View view, int startWidth, int endWidth, long duration) {
        ValueAnimator animator = ValueAnimator.ofInt(startWidth, endWidth);
        animator.addUpdateListener(animation -> {
            int animatedValue = (int) animation.getAnimatedValue();
            ViewGroup.LayoutParams layoutParams = view.getLayoutParams();
            layoutParams.width = animatedValue;
            view.setLayoutParams(layoutParams);
        });

        animator.setDuration(duration);
        animator.start();
    }

    private void openViewFromLeftToRight(View view, int targetWidth) {
        view.setVisibility(View.VISIBLE);
        animateHorizontalExpansion(view, 0, targetWidth, 800); // Expand from 0 to targetWidth
    }

    private void closeViewFromRightToLeft(View view) {
        int currentWidth = view.getWidth();
        animateHorizontalExpansion(view, currentWidth, 0, 800); // Collapse from current width to 0
    }
  
    View expandableView = layoutmoreoptionid;
                  int targetWidth = 1080; // Set your desired target width
                  openViewFromLeftToRight(expandableView, targetWidth);
  
