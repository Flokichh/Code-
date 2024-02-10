from django.contrib.auth import get_user_model
from django.views.decorators.csrf import csrf_exempt

User = get_user_model()

@csrf_exempt
def register(request):
    if request.method == 'POST':
        form = UserRegisterForm(request.POST)
        if form.is_valid():
            user = form.save()
            # generate token
            user.token = '...'  # Generate a token of 256 random characters
            user.save()
            return JsonResponse({ 'token': user.token })

@csrf_exempt
def create_order(request):
    token=request.META.get('HTTP_TOKEN')
    if token:
        user=User.objects.filter(token=token).first()
        if user is None:
            return JsonResponse({ 'error': 'Unauthorized' }, status=401)
        else:
            form = OrderForm(request.POST)
            if form.is_valid():
                order = form.save(commit=False)
                order.user = user
                order.save()
                return JsonResponse({ 'order_id': order.id })
